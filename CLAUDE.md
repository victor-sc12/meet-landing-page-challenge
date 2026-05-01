# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## AI Assistant Behavior

This project uses `AGENTS.md` as the source of truth for how to mentor the user. Read it before assisting — it defines the teaching style, hint progression, phrases to use/avoid, and escalation paths for a Newbie-level learner.

## Build Commands

```bash
npm run watch-sass      # compile SCSS and watch for changes (use during development)
npm run compile-sass    # one-time SCSS compilation
```

No linter or test suite is configured.

## Project Structure

The HTML lives in `starter-code/index.html`. Styles follow the **SCSS 7-1 pattern** and are compiled to `styles/css/style.css` — **never edit the CSS file directly**, only the SCSS source.

The stylesheet is linked from index.html as `../styles/css/style.css`, which means the `styles/` directory must stay at the root level alongside `starter-code/`.

### SCSS Architecture (7-1 Pattern)

`styles/scss/main.scss` is the entry point — it pulls in all partials via `@use`.

To use variables from `abstracts/` inside a layout partial, add `@use '../abstracts';` at the top and reference them as `abstracts.$variable-name`.

Assets in `starter-code/assets/` are pre-exported per breakpoint:
- `mobile/` — mobile images
- `tablet/` — tablet images  
- `desktop/` — desktop images (also used for the two hero images in the header)

## Challenge Requirements

Users need to build a responsive landing page matching the Figma design. The two user stories are:
1. Responsive layout adapts across mobile, tablet, and desktop screen sizes
2. Interactive elements show hover states

The HTML already uses BEM-style class naming (e.g., `site-header__image`, `main-content-header__title`). The SCSS file is nearly empty — the user writes the styles themselves.
