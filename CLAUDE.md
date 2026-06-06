# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

「星降る学園 ～放課後の約束～」— A browser-based visual novel (ビジュアルノベル) built as a single `index.html` file. No build tools, no dependencies, no server required.

## Running

Open `love-story/index.html` directly in a browser. No build step or dev server needed.

## Architecture

Everything lives in one self-contained HTML file with inline CSS and JavaScript:

- **CSS (lines 8–343):** Styles, animations (sakura petals, rain, star twinkle, text typing), and background gradients for each scene.
- **Character SVGs (lines 396–641):** Inline SVG sprite art for two characters — 陽菜 (Hina) and 蓮 (Ren).
- **Scene data (lines 644–860):** `SCENES` array defines the story as a linear sequence of scenes with branching via `next` pointers and `choice` options. Each scene has a day, background, optional effects, and dialogue steps.
- **Endings (lines 863–927):** `ENDINGS` object maps ending keys to scene data. Ending is determined by affection scores in `getEnding()`.
- **Game engine (lines 929–1237):** State machine driven by click events. Core loop: `playScene()` → `playStep()` → user click → advance. Choices modify `state.affection` and `state.flags`, which gate conditional dialogue and ending selection.

## Key Concepts

- **Affection system:** `state.affection.hina` and `state.affection.ren` are integers modified by player choices. Thresholds: ≥10 for true endings, ≥5 for normal endings.
- **Flags:** `state.flags` stores boolean flags set by choices (e.g., `helped_hina`, `chose_ren_day1`). Used in `condition` callbacks to show/hide dialogue lines.
- **Branching:** Some choices have a `next` property that jumps to a specific scene index; otherwise the scene's `next` field determines the next scene. `next: -1` triggers ending resolution.
- **Effects:** `sakura`, `rain`, and `stars` are particle/animation effects tied to scene backgrounds.

## Language

All game content is in Japanese. Code identifiers and comments are in English.
