# AGENTS.md

## Project purpose

This repository contains AI workshop and training materials rather than a traditional application codebase. The main artifacts are slide decks and related handouts:

- [ai-coding-workshop.md](ai-coding-workshop.md)
- [ai-architektur-agenta-kurs.md](ai-architektur-agenta-kurs.md)
- [ai-coding-workshop.html](ai-coding-workshop.html)

The repo is documentation-first. Most work is content editing, deck structure, slide formatting, and wording improvements.

## Working conventions

- Prefer updates to the existing Markdown slide sources instead of regenerating artifacts blindly.
- Preserve the Marp front matter at the top of the Markdown decks when editing slides.
- Keep the workshop tone and terminology consistent: mostly German, business/engineering language, and AI/architecture terminology used in the existing decks.
- Treat PDFs and image assets as reference content, not as sources to edit unless there is an explicit request.
- Do not invent a build or test pipeline for this repo unless a new requirement explicitly requires it.

## Content guidance

- Keep content clear, concise, and presentation-friendly.
- Maintain existing section structure, headings, and slide flow when making changes.
- When editing slides, prefer small, targeted changes over large rewrites.
- If a change affects terminology or governance advice, keep it aligned with the repo's AI/architecture themes and risk-aware messaging.

## Using agents

- Whenever a `.html` or `.htm` file is changed, run the `html-validator` subagent before finalizing the work.
- Treat HTML validation as a mandatory quality gate for deck updates, slide edits, asset changes, and navigation fixes.
- Do not mark an HTML change as complete without an explicit validation verdict: `OK`, `NOT OK`, or `PARTIAL`.
- Use the subagent for browser smoke testing, broken asset checks, and navigation validation; do not validate the page by inspection alone when a browser check is possible.

## Safety and scope

- Do not add or expose confidential, secret, or customer-specific data.
- Avoid assumptions about internal RAI policies beyond what is already written in the decks.
- If a request goes beyond documentation work, clarify the goal before editing project content.

## Preferred workflow for agents

1. Read the relevant slide deck or section before editing.
2. Make the smallest change that satisfies the request.
3. Keep formatting, slide structure, and narrative consistency intact.
4. If the change materially alters the workshop narrative, ensure the surrounding slides still fit the sequence.

This repository is optimized for documentation and presentation work, not for application development or CI automation.
