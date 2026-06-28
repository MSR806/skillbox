---
name: lean-docs
description: Use when writing, editing, or reviewing Markdown documentation; keeps docs lean, clear, non-repetitive, and human-friendly.
---

# Lean Docs

Write documentation for humans first. The reader should understand the point quickly without fighting verbosity, repetition, or structure noise.

## Core Rule

Say the useful thing once, clearly. Add detail only when it prevents confusion or misinterpretation.

## Writing Style

- Be direct and concise.
- Prefer short paragraphs.
- Use headings to organize, not decorate.
- Use bullets when they make scanning easier.
- Keep the document focused on what the reader needs to know.
- Avoid filler, throat-clearing, and generic introductions.
- Avoid long explanations when a precise sentence is enough.

## Structure

- Start with the main idea.
- Group related points together.
- Keep sections small and purposeful.
- Remove sections that repeat earlier sections.
- Use examples only when they clarify the point.
- Put operational details where the reader expects them.

## Repetition

Do not repeat the same information in multiple sections unless the repeat has a clear job, such as:

- reinforcing an important warning
- pointing back to a key decision
- summarizing a long section
- connecting two related ideas

If the repeat does not add value, delete it.

## Human-Friendly Constraint

LLMs tend to write too much. Cut the output until it is as short as possible while still being correct, complete, and hard to misread.

Good docs should not create reading fatigue. If a reader can safely skip a sentence, remove it.

## Editing Existing Docs

When cleaning an existing document:

- remove paste artifacts and formatting noise
- fix obvious grammar issues
- preserve the author's meaning
- keep the author's voice where possible
- collapse duplicate points
- prefer smaller edits over rewrites
- do not add new claims unless they are supported by the existing context

## Final Check

Before finishing, check:

- Is the main point obvious?
- Is anything repeated without purpose?
- Can any paragraph be shorter?
- Can any section be deleted?
- Could a human read this without fatigue?
- Is there enough context to avoid misinterpretation?

If the answer exposes extra bulk, cut it.
