---
name: slide-example-review
description: Use this skill when any presentation page or slide deck content changes and the agent must decide whether a new example is needed or whether an existing example should be adjusted to stay consistent with the current message. This includes deck edits, wording changes, technical examples, and workshop slides.
allowed-tools: Read, Grep, Search
---

# Slide Example Review

Review the changed slide/page and decide whether the existing examples still fit the message or whether a new example is needed.

## Goal

Keep the workshop content clear, consistent, and technically correct after any slide change.

## Decision rule

1. Read the changed section and identify the topic, audience, and intended message.
2. Check whether an existing example already explains the concept sufficiently.
3. If the example still matches the current narrative, keep it and adjust wording only if needed.
4. If the idea changed, the example is misleading, or the page now introduces a new concept, create a new example or revise the existing one.
5. Prefer small, presentation-friendly updates over large rewrites.

## Output expectations

Return:
- whether the current example is still valid
- whether the example should be kept, updated, or replaced
- a brief explanation of the decision
- a concrete example text or update suggestion when needed

## Guardrails

- Do not invent technical claims that are not already supported by the repo.
- Keep the workshop tone in German and business/engineering language.
- Maintain slide structure, terminology, and narrative flow.
- Prefer minimal edits that preserve the deck’s existing style.
