---
name: lean-expert-workspace-manager
description: >-
  Guidelines, architecture rules, and workflows for developing, maintaining,
  and synchronizing tools in the Masa Lean Expert Workspace (8D Problem Solving,
  Process Cpk Simulator, SMART Principles, Wafer Yield Calculator, Gage R&R,
  Statistical Significance, Quality Statistics Hub, Define Limit SOP).
---

# Lean Expert Workspace Development & Maintenance Skill

This skill documents the complete architecture, UI/UX design standards, multi-file synchronization protocols, DOM integrity rules, JavaScript validation workflows, and feature guidelines for the **Masa Lean Expert Workspace** application suite.

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

## 4. Strict CSS Scoping & Isolation Standard

- **Scope Everything**: All tool-specific styles MUST be scoped under their respective CSS class prefix (e.g. `.lim-scope`, `.smart-scope`, `.sc-scope`, `.pr-scope`).
- **No Unscoped Generic Selectors**: Never write unscoped `.card`, `.btn`, `.preset-btn`, `.form-group`, `table`, `input`, or `header` in global CSS.
- **Dynamic Palette Binding**: Utilize core CSS variables to ensure seamless light/dark mode support:
  - `var(--bg-primary)` / `var(--bg-secondary)` / `var(--bg-tertiary)`
  - `var(--border-color)`
  - `var(--text-primary)` / `var(--text-muted)`
  - `var(--color-blue)` / `var(--color-emerald)` / `var(--color-amber)` / `var(--color-rose)`

---

## 5. MathJax Performance & DOM Scanning Rule

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

## 6. Header, Identity & Metadata Standards

Every workspace file must maintain:
- **Title & Author Attribution**: `Masa Lean expert workingspace @ Masa Tu`
  - Email: `mailto:masahltu0322@gmail.com`
  - LinkedIn: `https://www.linkedin.com/in/masatu19810322/`
- **MASA TU Motto Banner**:
  `M 挑戰精進 (Mastery Challenge) · A 目標對齊 (Align & Adjust) · S 解決問題 (Solve Problems) · A 迅速行動 (Act Swiftly) · T 團隊協作 (Team Up) · U 成就他人 (Uplift Others)`
- **Version & Update Badge**: Next to theme toggle (e.g. `v1.4.1 | Update: YYYY-MM-DD`).

---

## 7. Dual-Theme & Eye-Care Design Standard (護眼雙模式)

- **Light Mode**:
  - `--bg-primary: #f1f5f9;` (Soft slate, never blinding `#ffffff`).
  - `--bg-secondary: #ffffff;` (Cards & containers).
  - `--border-color: #e2e8f0;` (Hairline border).
  - `--text-primary: #1e293b;` (Deep slate for crisp contrast).
  - `--color-blue: #0284c7;` (Legible non-glaring sky blue).
  - Header: Solid `#ffffff` with subtle soft shadow (never hardcoded dark gradients).
- **Dark Mode**: Rich slate `#0b0f19` / `#151c2c`, cyan/purple accents.
- **Plotly Chart Synchronization**: Automatically sync Plotly chart background, text, and grid colors on theme switch.

---

## 8. Git Operations & Automation Rule

- Authorized to perform Git actions (stage, commit, push to `origin main`) immediately upon completing and verifying code modifications without waiting for manual confirmation.
