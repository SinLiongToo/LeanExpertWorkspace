# Agent Rules for Masa Lean Expert Workspace

## 1. Git Automation
- You do not need to stop and ask for permission or approval for Git activities (such as staging, committing, or pushing). You are authorized to run Git commands directly.
- Always commit and push changes immediately after code modifications are verified.

## 2. Multi-File Synchronization Rule
Whenever modifying headers, styles, adding tabs/tools, or fixing bugs, ALWAYS synchronize all 4 core workspace files:
1. `index.html` (Primary engineering workbench)
2. `CPKn SIMULATOR.html` (Cpk simulator variant)
3. `Masa Lean expert workingspace @ Masa Tu.html` (Master production mirror)
4. `8d_problem solving.html` (8D problem solving variant)

## 3. DOM Integrity & Anti-Duplication Rule
- When integrating standalone HTML tools, **never** concatenate whole HTML files.
- Strip standalone outer headers, redundant mottos, and redundant theme buttons. Embed solely the inner tool panel inside `<div id="...Panel" class="panel hidden <scope>-scope">`.
- Guarantee that every workspace file has strictly **1** `<!DOCTYPE html>`, **1** `<header>`, **1** `<body>`, and **1** `#mainContainer`.

## 4. Mandatory JavaScript AST Validation Before Commit
- Always execute an automated JavaScript syntax check via Node.js on all `<script>` tags across all 4 files before committing.
- Ensure 0 syntax errors (`SyntaxError`), verify balanced braces in `switchTab`, and confirm that all critical entry functions (`recalculate`, `smartInit`, `prInit`, `scCalculate`, `runCpkSimulation`, `toggleDashboardTheme`) are defined and intact.

## 5. Dual-Theme Architecture & CSS Scoping Requirement
- The workspace theme is controlled via `data-theme` on `<html>` (`document.documentElement.getAttribute('data-theme')` with `'dark'` default and `'light'`). **NEVER** use `body.dark-mode` or `classList.contains('dark-mode')`.
- In JS (for Plotly / Canvas / SVG), theme check MUST be: `const isDark = document.documentElement.getAttribute('data-theme') !== 'light';`.
- All tool-specific CSS rules MUST be scoped under their respective scope class (e.g. `.lim-scope`, `.smart-scope`, `.sc-scope`, `.pr-scope`).
- Scope variables must bind to core tokens: `--bg-primary`, `--bg-secondary` (cards/tables), `--bg-tertiary` (sub-cards/inputs), `--border-color`, `--text-primary`, `--text-muted`, `--color-blue`.
- Light mode overrides MUST use `[data-theme="light"] .<scope> { ... }`.
- Never introduce unscoped global selectors like `.card`, `.preset-btn`, `.form-group`, `input`, `table`, `th`, `td` that could collide with other tabs.

## 6. MathJax Performance & DOM Scanning Rule
- Keep exactly ONE optimized, asynchronous MathJax script tag in `<head>` with `options: { skipHtmlTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code'] }` to eliminate UI blocking and page lag.

## 7. Header & Attribution Standards
- Maintain author attribution `@ Masa Tu` with working hyperlinks:
  - Email: `mailto:masahltu0322@gmail.com`
  - LinkedIn: `https://www.linkedin.com/in/masatu19810322/`
- Preserve the MASA TU motto banner under the main title.
- Keep the version tag and update date badge in the header next to the theme toggle button (e.g., `v1.4.1 | Update: YYYY-MM-DD`).

## 8. Eye-Care Dual-Theme Standards (護眼雙模式)
- **Light Mode**: Must be eye-friendly and glare-free:
  - Background: Soft slate `--bg-primary: #f1f5f9;` (never blinding pure white `#ffffff`).
  - Cards & Panels: Clean white `#ffffff` with hairline border `--border-color: #e2e8f0;`.
  - Header: Solid `#ffffff` background with subtle soft shadow `rgba(15, 23, 42, 0.04)` (never hardcoded dark gradients).
  - Blue Accents & Motto: Non-glaring legible blue `#0284c7` (never neon light-cyan `#38bdf8` on white).
  - Plotly Charts: Automatically synchronized with theme background, text `#1e293b`, and grid `#e2e8f0`.
- **Dark Mode**: High contrast, rich slate `#0b0f19` / `#151c2c`, cyan/purple accents.

## 9. Tool & Tab Architecture Standard
When adding new tools or modules (such as SMART Principles, Wafer Yield, MSA, Limit SOP):
- Provide a three-tier sub-tab experience:
  1. Interactive Tool / Calculator / Evaluator
  2. Demo / Mock Library with 1-click loading and comparison
  3. Comprehensive HELP / Guide / Best Practice Knowledge Base
- Ensure seamless integration with 8D tasks (`kanbanTasks`) when applicable.
- Register tool redraw functions in `toggleDashboardTheme()` for instant chart color synchronization upon theme switching.
