---
inclusion: always
name: plugins
description: Enabled Obsidian plugins and the Markdown syntax they support.
---
# Enabled Plugins and Syntax

This vault has several community and core plugins enabled. When writing
notes, prefer the syntax these plugins understand so content renders
correctly.

## Core Plugins

- Daily notes and Templates are enabled. When asked for a daily note or
  a templated note, follow any existing template structure in the vault.
- Bases, Canvas, Graph, Backlinks, Outline, and Tag pane are enabled.
- Slides are enabled: separate slides with `---` on its own line when
  authoring a presentation note.

## Community Plugins

- obsidian-git: the vault is a git repository. Do not commit unless
  asked.
- dataview: use Dataview query blocks (```` ```dataview ````) and inline
  `= this.field` queries when the user wants dynamic lists or tables.
- obsidian-latex-suite: math uses LaTeX. Inline math is `$...$`; block
  math uses `$$...$$` (a block requires at least two dollar signs).
- mermaid-tools: diagrams use Mermaid fenced blocks
  (```` ```mermaid ````).
- obsidian-excalidraw-plugin: hand-drawn diagrams are stored as
  Excalidraw notes; do not hand-edit their JSON.
- obsidian-tagfolder: tags organize notes. Use `#tag` or nested
  `#parent/child` tags; keep tag names consistent.
- obsidian-kanban: Kanban boards are special Markdown notes; edit them
  through their list structure, not raw internals, unless asked.
- table-editor-obsidian and obsidian-excel-to-markdown-table: keep
  Markdown tables well-formed with aligned pipes.
- obsidian-map-view: notes may carry location front matter; preserve it.
- obsidian-linter: formatting is auto-corrected on save (see the vault
  conventions steering file for the active rules).
- obsidian-languagetool-plugin: grammar checking is available; write
  clear prose.
