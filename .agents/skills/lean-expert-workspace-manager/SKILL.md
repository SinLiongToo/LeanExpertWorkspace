---
name: lean-expert-workspace-manager
description: >-
  Guidelines, architecture rules, and workflows for developing, maintaining,
  and synchronizing tools in the Masa Lean Expert Workspace (8D Problem Solving,
  Process Cpk Simulator, SMART Principles, Wafer Yield Calculator, Gage R&R,
  Statistical Significance, Quality Statistics Hub).
---

# Lean Expert Workspace Development & Maintenance Skill

This skill documents the complete architecture, UI/UX design standards, multi-file synchronization protocols, and feature guidelines for the **Masa Lean Expert Workspace** application suite.

## Workspace Core Files & Multi-File Synchronization

Any UI change, new tool/tab addition, bug fix, or style modification **MUST** be synchronized across all core workspace HTML files:

1. `index.html` - Primary comprehensive single-page engineering workbench.
2. `CPKn SIMULATOR.html` - Dedicated process capability simulator variant.
3. `Masa Lean expert workingspace @ Masa Tu.html` - Master production mirror.
4. `8d_problem solving.html` - Specialized 8D Problem Solving and statistical toolkit.

> **Rule**: When editing or introducing features, always check and apply the changes across all 4 files to maintain 100% consistency.

---

## Header, Identity & Metadata Standards

Every workspace file must maintain the standardized header structure:

### 1. Title & Author Attribution
- **Title**: `Masa Lean expert workingspace @ Masa Tu`
- **Contact Info Hyperlinks**:
  - Email: `<a href="mailto:masahltu0322@gmail.com">masahltu0322@gmail.com</a>`
  - LinkedIn: `<a href="https://www.linkedin.com/in/masatu19810322/" target="_blank">https://www.linkedin.com/in/masatu19810322/</a>`
  - Styled with `.contact-info` to prevent gradient text clipping.

### 2. MASA TU Motto Banner
```html
<p id="mottoHeader" style="font-size: 11.5px; color: var(--color-blue); font-weight: 500; margin-top: 2px; letter-spacing: 0.2px;">
    M 挑戰精進 (Mastery Challenge) · A 目標對齊 (Align & Adjust) · S 解決問題 (Solve Problems) · A 迅速行動 (Act Swiftly)   ·   T 團隊協作 (Team Up) · U 成就他人 (Uplift Others)
</p>
```

### 3. Version & Update Badge
- Located inside `.header-actions` alongside the Theme Toggle Button (`#themeToggleBtn`):
```html
<div class="version-badge" style="display: flex; align-items: center; gap: 8px; font-size: 11.5px; font-family: var(--font-mono, monospace); background: var(--bg-tertiary); border: 1px solid var(--border-color); color: var(--text-muted); padding: 7px 12px; border-radius: 6px; user-select: none;">
    <span style="display: inline-flex; align-items: center; gap: 4px; color: var(--color-blue); font-weight: 700;">
        <span style="display: inline-block; width: 6px; height: 6px; border-radius: 50%; background: var(--color-emerald); box-shadow: 0 0 6px var(--color-emerald);"></span>
        v1.3.0
    </span>
    <span style="opacity: 0.4;">|</span>
    <span>Update: 2026-09-01</span>
</div>
```

---

## Dual-Theme & Eye-Care Design Standard (護眼雙模式規範)

### 1. Dark Mode Palette (Default)
- `--bg-primary: #0b0f19;`
- `--bg-secondary: #151c2c;`
- `--bg-tertiary: #1e293b;`
- `--border-color: #26354a;`
- `--text-primary: #f8fafc;`
- `--text-muted: #94a3b8;`
- `--color-blue: #38bdf8;` (Cyan-glow accent)

### 2. Light Mode Palette (Eye-Care & Anti-Glare Overrides)
To prevent eye fatigue and screen glare:
- `--bg-primary: #f1f5f9;` (Soft slate background — **never** use stark blinding `#ffffff` as page background).
- `--bg-secondary: #ffffff;` (Clean card container background).
- `--bg-tertiary: #f8fafc;` / `#e2e8f0;` (Soft card headers and sub-tabs).
- `--border-color: #e2e8f0;` (Gentle hairline border).
- `--text-primary: #1e293b;` (Deep slate-800 — comfortable, crisp readability).
- `--text-muted: #64748b;` (Slate-500).
- `--color-blue: #0284c7;` (Sky-600 — calibrated, non-glaring blue).
- `--color-emerald: #059669;`, `--color-purple: #7c3aed;`, `--color-red: #e11d48;`
- **Header**: Solid `#ffffff` background with subtle soft shadow `rgba(15, 23, 42, 0.05)`, **never** hardcoded dark gradients!

### 3. Dynamic Plotly Chart Theme Synchronization
```javascript
function getPlotlyColors() {
    const isLight = document.documentElement.getAttribute('data-theme') === 'light';
    return {
        text: isLight ? '#1e293b' : '#f8fafc',
        muted: isLight ? '#64748b' : '#94a3b8',
        grid: isLight ? '#e2e8f0' : '#26354a'
    };
}
```

---

## Tab Architecture & Module Development Standard

When adding a new engineering or quality tool tab:

### 1. Scoped CSS & Namespace
- Wrap all styles in a dedicated class scope (e.g. `.smart-scope`, `.sc-scope`, `.pr-scope`) to guarantee zero interference with existing tabs.

### 2. Three-Tier Sub-Tab Convention
Every major tool should feature:
1. **Interactive Tool / Evaluator / Calculator** (Core computational/workflow interface).
2. **Demo / Mock Library** (Preset realistic industrial scenarios with 1-click loading).
3. **HELP / Guide / FAQ** (Theoretical background, formulas, checklists, best practices).

### 3. Integration with 8D Problem Solving
- Provide seamless action export (e.g., adding evaluated SMART action targets directly into `kanbanTasks` with phase `'D6'`).

---

## Git Operations & Automation Rule
- Automatic Git commits and push to `origin main` upon completing and verifying code changes without requiring additional confirmation.
