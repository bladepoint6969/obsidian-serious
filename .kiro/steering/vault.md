---
inclusion: always
name: vault
description: Core conventions for this Obsidian vault.
---
# Obsidian Vault Conventions

This workspace is an Obsidian vault, not a code project. Notes are
Markdown files (`.md`) linked together with wikilinks. Treat every note
as user-authored knowledge and preserve the author's voice when editing.

## Markdown Rules in This Vault

The standard Markdown steering rules still apply to notes, with two
deliberate deviations for this vault. Where a rule below conflicts with
the standard rules, the rule below wins, because it reflects how
Obsidian and the obsidian-linter actually behave.

- Do not hard-wrap prose at 80 columns. Obsidian treats each paragraph
  as a single soft-wrapped line, and the linter runs on save, so hard
  wrapping fights the editor. Write one logical paragraph per line and
  let it wrap. (The 80-column rule still applies to the steering files
  in `.kiro/steering/`, which are edited as plain Markdown.)
- Do not enforce unique heading text. The standard rules require every
  heading to be unique across a document; Obsidian does not need this
  and generates working anchors for duplicate headings, which are common
  in notes.

Everything else from the standard Markdown rules applies unchanged: ATX
headings, `-` for unordered lists, 2-space nested indentation, fenced
code blocks with language identifiers, `*`/`**` for emphasis (not
underscores), trailing-backslash line breaks (not trailing spaces),
padded tables, straight ASCII quotes, no trailing whitespace, and a
single trailing newline.

## Note Structure

- Each note starts with a single H1 that matches the file name. The
  obsidian-linter `file-name-heading` rule enforces this, so when you
  create a note named `Project Plan.md`, its first line must be
  `# Project Plan`.
- Do not skip heading levels. Headings increment by one
  (`#` then `##` then `###`); the `header-increment` rule enforces this.
- Headings must not have trailing punctuation (`. , ; : !` and their
  fullwidth variants are stripped by `remove-trailing-punctuation-in-heading`).
- The first heading sits at the top of the note (no leading blank lines),
  per `headings-start-line`.

## Links and Attachments

- Use Obsidian wikilinks for internal references: `[[Note Name]]` or
  `[[Note Name|display text]]`. Prefer these over Markdown links for
  vault-internal navigation.
- New links use the relative path format (`newLinkFormat: relative`).
- Store images and other attachments in the `_assets` folder
  (`attachmentFolderPath: _assets`). Reference them with wikilinks such
  as `![[diagram.png]]`.
- Links are kept up to date automatically on rename
  (`alwaysUpdateLinks`), so renaming a note is safe.

## Formatting

- Indent with 2 spaces. Never use tabs (`useTab: false`, `tabSize: 2`).
- The linter runs on save and on file change (`lintOnSave`,
  `lintOnFileChange`), so keep edits consistent with the rules above to
  avoid churn.
- Locale is English (Canada) (`en-ca`); prefer Canadian spelling.

## Editing Etiquette

- Do not delete or rewrite existing notes wholesale unless asked. Make
  targeted edits and keep the author's structure.
- The `Welcome.md` note is Obsidian's default placeholder; it can be
  removed or replaced freely.
- This vault is version-controlled with the obsidian-git plugin. Do not
  create commits unless explicitly asked.
