---
name: lean-expert-workspace-manager
description: >-
  Guidelines, architecture rules, and workflows for developing, maintaining,
  and synchronizing tools in the Masa Lean Expert Workspace (8D Problem Solving,
  Process Cpk Simulator, SMART Principles, Wafer Yield Calculator, Gage R&R,
  Statistical Significance, Quality Statistics Hub, Define Limit SOP).
---

# Lean Expert Workspace Development & Maintenance Skill

This skill documents the complete architecture, UI/UX design standards, multi-file synchronization protocols, DOM integrity rules, JavaScript validation workflows, theme binding rules, and feature guidelines for the **Masa Lean Expert Workspace** application suite.

---

## 1. Workspace Core Files & Multi-File Synchronization

Any UI change, new tool/tab addition, bug fix, or style modification **MUST** be synchronized across all core workspace HTML files:

1. `index.html` - Primary comprehensive single-page engineering workbench.
2. `CPKn SIMULATOR.html` - Dedicated process capability simulator variant.
3. `Masa Lean expert workingspace @ Masa Tu.html` - Master production mirror.
4. `8d_problem solving.html` - Specialized 8D Problem Solving and statistical toolkit.

> **Rule**: When editing or introducing features, always check and apply changes across all 4 files to maintain 100% consistency.

---

## 2. DOM Integrity & Single-Container Standard (Anti-Duplication Protocol)

When embedding standalone tools (e.g., Define Limit SOP, SMART Principles, Wafer Yield Calculator):
- **Never Concatenate Entire HTML Documents**: Do not append or duplicate `<!DOCTYPE html>`, `<html>`, `<head>`, `<body>`, `<header>`, or outer `.dashboard-container` tags.
- **Strip Standalone Outer Headers & Theme Buttons**: Embedded tools must use the workspace's global header, attribution banner, and theme switcher. Do not introduce redundant inner motto banners or inner dark mode buttons.
- **Embed as Scoped Panel**: Wrap tool content inside a dedicated panel:
  ```html
  <div id="<tool>Panel" class="panel hidden <tool>-scope">
      <!-- Tool interface without redundant outer headers -->
  </div>
  ```
- **Single-DOM Verification**: Every workspace file must strictly contain:
  - Exactly **1** `<!DOCTYPE html>`
  - Exactly **1** `<header>`
  - Exactly **1** `<body>`
  - Exactly **1** `#mainContainer` (`.dashboard-container`)

---

## 3. Mandatory JavaScript AST Validation (Zero-Syntax-Error Protocol)

To prevent breaking click handlers and icons:
- **Pre-Commit Automated Syntax Check**: Before committing or finalizing any change, run an automated AST syntax check with Node.js on all inline scripts across all 4 files:
  ```bash
  node -e "
  const fs = require('fs');
  const files = ['index.html', 'CPKn SIMULATOR.html', 'Masa Lean expert workingspace @ Masa Tu.html', '8d_problem solving.html'];
  files.forEach(f => {
    const html = fs.readFileSync(f, 'utf8');
    const scripts = html.match(/<script[\s\S]*?<\/script>/gi) || [];
    scripts.forEach((s, i) => {
      if (s.includes('src=')) return;
      const js = s.replace(/<script[^>]*>/i, '').replace(/<\/script>/i, '');
      try { new Function(js); } catch(e) { console.error('ERROR in ' + f + ' script ' + i + ':', e.message); process.exit(1); }
    });
  });
  console.log('ALL JS SCRIPTS 100% VALID');
  "
  ```
- **switchTab Bracket Integrity**: Ensure all `if ... else if` branches inside `switchTab(tabId)` have balanced braces and correctly toggle panels, sidebars, and `.full-width` containers.
- **Function Integrity**: Verify that essential module entrypoints (`switchTab`, `toggleDashboardTheme`, `recalculate`, `smartInit`, `prInit`, `scCalculate`, `runCpkSimulation`, etc.) remain intact and un-truncated.

---

## 4. Dual-Theme System Architecture & Variable Binding Standard

The workspace uses `document.documentElement.getAttribute('data-theme')` (values: `'dark'` [default] or `'light'`), **NEVER** `body.dark-mode`.

### A. JavaScript Theme Detection Standard
In any interactive module or Plotly chart generator:
```javascript
// ✅ ALWAYS USE THIS (works with default dark and toggled light modes):
const isDark = document.documentElement.getAttribute('data-theme') !== 'light';

// ❌ NEVER USE THIS (will fail in workspace dark mode):
// const isDark = document.body.classList.contains('dark-mode');
```

### B. Plotly Layout Dynamic Palette
```javascript
const paperBg = isDark ? '#151c2c' : '#ffffff';
const plotBg = isDark ? '#151c2c' : '#ffffff';
const fontColor = isDark ? '#cbd5e1' : '#334155';
const gridColor = isDark ? '#273553' : '#e2e8f0';
```

### C. CSS Theme Variable Mapping
All tool-scoped CSS blocks (`.lim-scope`, `.smart-scope`, `.pr-scope`, `.sc-scope`) must bind directly to workspace core variables:
```css
.<tool>-scope {
  --bg-primary: var(--bg-primary);       /* Dark: #0b0f19, Light: #f1f5f9 */
  --bg-card: var(--bg-secondary);        /* Dark: #151c2c, Light: #ffffff */
  --bg-subtle: var(--bg-tertiary);       /* Dark: #1e293b, Light: #f8fafc */
  --border-color: var(--border-color);   /* Dark: #26354a, Light: #e2e8f0 */
  --border-focus: var(--color-blue);
  --text-main: var(--text-primary);      /* Dark: #f8fafc, Light: #1e293b */
  --text-muted: var(--text-muted);       /* Dark: #94a3b8, Light: #64748b */
  --text-highlight: var(--color-blue);
  --accent-blue: var(--color-blue);
  --accent-green: var(--color-emerald);
  --accent-amber: var(--color-orange, #f59e0b);
  --accent-rose: var(--color-red, #ef4444);
  --accent-purple: var(--color-purple);
  --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.25);
  --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.35);
  color: var(--text-main);
}

[data-theme="light"] .<tool>-scope {
  --shadow-sm: 0 1px 3px rgba(15, 23, 42, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(15, 23, 42, 0.07);
}
```

### D. Theme Switcher Hook (`toggleDashboardTheme`)
Ensure every active tab's redraw function is registered in `toggleDashboardTheme()`:
```javascript
if (activeTab === 'limitsop' && typeof recalculate === 'function') recalculate();
else if (activeTab === 'smart' && typeof smartRenderRadar === 'function') smartRenderRadar();
else if (activeTab === 'yield' && typeof scUpdateViz === 'function') { scUpdateViz(); scDrawMiniWafers(); }
```

---

## 5. CSS Scoping & Layout Fidelity Standard

- **100% Selector Scoping**: When integrating external tools, extract all CSS selectors and prefix each with `.<tool>-scope ` (e.g. `.<tool>-scope .kpi-card`, `.<tool>-scope table`, `.<tool>-scope th`, `.<tool>-scope td`).
- **No Class Name Guessing**: Preserve the exact class names used in the HTML (e.g., `.kpi-card`, `.kpi-grid`, `.table-container`, `.preset-btn`) to ensure cards, KPI metrics, tables, and buttons render with full formatting.
- **No Global Leakage**: Never write unscoped global selectors (`.card`, `.btn`, `.form-group`, `input`, `select`, `table`) that could override other tabs.

---

## 6. MathJax Performance & DOM Scanning Rule

- Only include **ONE** MathJax script tag in `<head>`.
- Configure MathJax with `skipHtmlTags` to prevent scanning pre/code/textarea elements and blocking UI interaction:
  ```html
  <script>
    window.MathJax = {
      tex: { inlineMath: [['$', '$'], ['\\(', '\\)']], displayMath: [['$$', '$$'], ['\\[', '\\]']] },
      options: { skipHtmlTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code'] },
      startup: { pageReady: () => MathJax.startup.defaultPageReady() }
    };
  </script>
  <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
  ```

---

## 7. Header, Identity & Metadata Standards

Every workspace file must maintain:
- **Title & Author Attribution**: `Masa Lean expert workingspace @ Masa Tu`
  - Email: `mailto:masahltu0322@gmail.com`
  - LinkedIn: `https://www.linkedin.com/in/masatu19810322/`
- **MASA TU Motto Banner**:
  `M 挑戰精進 (Mastery Challenge) · A 目標對齊 (Align & Adjust) · S 解決問題 (Solve Problems) · A 迅速行動 (Act Swiftly) · T 團隊協作 (Team Up) · U 成就他人 (Uplift Others)`
- **Version & Update Badge**: Next to theme toggle (e.g. `v1.4.1 | Update: YYYY-MM-DD`).

---

## 8. Git Operations & Automation Rule

- Authorized to perform Git actions (stage, commit, push to `origin main`) immediately upon completing and verifying code modifications without waiting for manual confirmation.
