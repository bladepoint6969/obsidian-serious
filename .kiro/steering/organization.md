---
inclusion: always
name: organization
description: How notes are organized in this vault: folders, tags, and links.
---
# Note Organization

This vault uses a flat layout with tags and links as the primary way to
organize and connect notes. Structure emerges from metadata and
relationships, not from a deep directory tree.

## Folders

Folders exist only to separate kinds of files by function, not by
subject. Keep this skeleton small:

- Notes live flat at the vault root (no topical subfolders).
- `daily/` holds daily notes.
- `templates/` holds note templates.
- `_assets/` holds images and other attachments.

Do not create subject-based folders (e.g. by topic or project). Sorting
by subject is the job of tags and links, described below.

## Tags

Tags are the detailed sorting system. They classify notes by topic,
status, type, or any other facet.

- Place tags at the bottom of the note, on their own line, as the last
  content in the file. Do not put tags in YAML front matter and do not
  scatter them inline through the body.
- Use nested tags for hierarchy where it helps (e.g. `#topic/subtopic`,
  `#status/active`).
- Keep tag names consistent to avoid sprawl. Reuse an existing tag
  rather than coining a near-duplicate (avoid `#project` vs `#projects`
  vs `#proj`). When unsure which tag applies, check tags already used
  elsewhere in the vault first.
- No fixed taxonomy is defined yet; it will emerge as notes accumulate.

## Links

- Connect related notes with wikilinks (`[[Note Name]]`) rather than
  relying on folders to group them.
- As clusters of related notes form, create a Map of Content (MOC): a
  hub note that links out to the notes on a topic. Prefer MOCs over
  folders for "show me everything about X" navigation.

## Dynamic Views

- Use dataview queries to generate lists and tables of notes by tag,
  link, or field, instead of maintaining such lists by hand.
- tagfolder renders a tree from tags, so a good tag habit gives you a
  folder-like view without physical folders.
