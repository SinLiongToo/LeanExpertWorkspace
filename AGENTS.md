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

## 3. Header & Attribution Standards
- Maintain author attribution `@ Masa Tu` with working hyperlinks:
  - Email: `mailto:masahltu0322@gmail.com`
  - LinkedIn: `https://www.linkedin.com/in/masatu19810322/`
- Preserve the MASA TU motto banner under the main title.
- Keep the version tag and update date badge in the header next to the theme toggle button (e.g., `v1.3.0 | Update: YYYY-MM-DD`).

## 4. Eye-Care Dual-Theme Standards (護眼雙模式)
- **Light Mode**: Must be eye-friendly and glare-free:
  - Background: Soft slate `--bg-primary: #f1f5f9;` (never blinding pure white `#ffffff`).
  - Cards & Panels: Clean white `#ffffff` with hairline border `--border-color: #e2e8f0;`.
  - Header: Solid `#ffffff` background with subtle soft shadow `rgba(15, 23, 42, 0.04)` (never hardcoded dark gradients).
  - Blue Accents & Motto: Non-glaring legible blue `#0284c7` (never neon light-cyan `#38bdf8` on white).
  - Plotly Charts: Automatically synchronized with theme background, text `#1e293b`, and grid `#e2e8f0`.
- **Dark Mode**: High contrast, rich slate `#0b0f19` / `#151c2c`, cyan/purple accents.

## 5. Tool & Tab Architecture Standard
When adding new tools or modules (such as SMART Principles, Wafer Yield, MSA):
- Scope CSS under a dedicated namespace (e.g. `.smart-scope`) to avoid style bleeding.
- Provide a three-tier sub-tab experience:
  1. Interactive Tool / Calculator / Evaluator
  2. Demo / Mock Library with 1-click loading and comparison
  3. Comprehensive HELP / Guide / Best Practice Knowledge Base
- Ensure seamless integration with 8D tasks (`kanbanTasks`) when applicable.
