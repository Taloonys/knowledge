# knowledge
* Well, it's just a simple personal notes (tbh I've never rechecked them, but some my friends did... idk for what).
* Here I described my associations with some themes (mostly it's in C++) and shortened some term descriptions.
* Most of the notes are in russian

# Navigation (MOC-first)
- Start at `mocs/INDEX.md` for the vault map.
- `mocs/IT.md` links all tech areas (programming, system design, DB, frontend, network, ML, gamedev).
- `mocs/COMMON.md` keeps non-IT topics; `src/common/common.md` keeps computing basics.
- Links stay as standard markdown (`[name](route.md)`) to stay git-friendly.

# Structured like...
- all notes live in `src/` (programming, system-design, db, frontend, network, ML, etc.)
- `mocs/` holds navigation maps (MOCs).
- `images/` is flat; files are prefixed with their topic (e.g., `images/system-design__*.png`).
- topic entry files are named after their area (e.g., `src/databases/databases.md`, `src/code/code.md`); individual notes live alongside them.

 # Designed in obsidian, but ok for just git reading
* Settings for obsidian (It makes files readable in git raw markdown view)
  - Options -> Files and Links -> Use [Wikilinks](Wikilinks) -> "False" (not sure, but looks like sth changed...)
  - Options -> Files and Links -> New link format   -> "Relative path to file"
  - Options -> Editor          -> Advanced          -> Convert pasted HTML to Markdown -> "True"
