B.2 Shortcut Cheatsheet

# Shortcut Cheatsheet

The following is a summary of the main Emacs Live shortcuts available within Sonic Pi. Please see Section B.1 for motivation and background.

## Conventions

In this list, we use the following conventions (where *Meta* is one of *Alt* on Windows/Linux or *Cmd* on Mac):

* `C-a` means hold the *Control* key then press the *a* key whilst holding them both at the same time, then releasing.
* `M-r` means hold the *Meta* key and then press the *r* key whilst holding them both at the same time, then releasing.
* `S-M-z` means hold the *Shift* key, then the *Meta* key, then finally the *z* key all at the same time, then releasing.
* `C-M-f` means hold the *Control* key, then press *Meta* key, finally the *f* key all at the same time, then releasing.

## Main Application Manipulation

* `M-r` - Run code
* `M-s` - Stop code
* `M-i` - Toggle Help System
* `M-p` - Toggle Preferences
* `M-{` - Switch buffer to the left
* `M-}` - Switch buffer to the right
* `S-M-0` - Switch to buffer 0
* `S-M-1` - Switch to buffer 1
* ...
* `S-M-9` - Switch to buffer 9
* `M-+` - Increase text size of current buffer
* `M--` - Decrease text size of current buffer


## Merket område/Kopier/Lim inn

* `M-a` - Merk alt
* `M-c` - Kopier merket omrade til innlimingsmellomlager
* `M-]` - Kopier merket omrade til innlimingsmellomlager
* `M-x` - Klipp ut merket område til innlimingsmellomlager
* `C-]` - Klipp ut merket område til innlimingsmellomlager
* `C-k` - Klipp ut til slutten av linjen
* `M-v` - Lim in fra innlimingsmellomlageret til tekstredigeringsfeltet
* `C-y` - Lim in fra innlimingsmellomlageret til tekstredigeringsfeltet
* `C-SPACE` - Sett merke. Navigering vil nå manipulere uthevet område. Bruk `C-g` for å komme ut.

## Tekstmanipulering

* `M-m` - Align all text
* `Tab` - Align current line or selection (or select autocompletion)
* `C-l` - Centre editor
* `M-/` - Kommenter/avkommenter gjeldende linje eller merket område
* `C-t` - Transpose/swap characters
* `M-u` - Gjør om neste ord (eller utvalg) til store bokstaver.
* `M-l` - Gjør om neste ord (eller utvalg) til små bokstaver.

## Navigation

* `C-a` - Flytt til starten av linjen
* `C-e` - Flytt til slutten av linjen
* `C-p` - Flytt til forrige linje
* `C-n` - Flytt til neste linje
* `C-f` - Flytt forover ett tegn
* `C-b` - Flytt bakover ett tegn
* `M-f` - Flytt forover ett ord
* `M-b` - Flytt bakover ett ord
* `C-M-n` - Flytt linje eller utvalg ned
* `C-M-p` - Flytt linje eller utvalg opp
* `S-M-u` - Flytt opp 10 linjer
* `S-M-d` - Flytt ned 10 linjer
* `M-<` - Move to beginning of buffer
* `M->` - Move to end of buffer

## Sletting

* `C-h` - Delete previous character
* `C-d` - Delete next character

## Advanced Editor Features

* `C-i` - Show docs for word under cursor
* `M-z` - Undo
* `S-M-z` - Redo
* `C-g` - Escape
* `S-M-f` - Toggle fullscreen mode
* `S-M-b` - Toggle visibility of buttons
* `S-M-l` - Toggle visibility of log
* `S-M-m` - Toggle between light/dark modes
* `S-M-s` - Save contents of buffer to a file
* `S-M-o` - Load contents of buffer from a file
