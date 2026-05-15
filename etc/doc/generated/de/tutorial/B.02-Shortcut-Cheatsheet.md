10.2 Cheatsheet für Tastaturkürzel

# Cheatsheet für Tastaturkürzel

The following is a summary of the main Emacs Live shortcuts available within Sonic Pi. Please see Section B.1 for motivation and background.

## Konventionen

Wir verwenden die folgenden Konventionen (*Meta* ist *Alt* auf Windows und Linux und *Cmd* auf dem Mac wie in 10.1 beschrieben):

* `C-a` bedeutet, die *Control* (*Strg* auf deutschen Tastaturen) zu drücken und zu halten, dabei *a* zu drücken und danach beide Tasten wieder los zu lassen.
* `M-r` bedeutet, *Meta* zu drücken und zu halten, dabei die *r*-Taste zu drücken und danach beide wieder los zu lassen.
* `S-M-z` bedeutet, die *Meta*-Taste zu drücken und zu halten, dann *Shift* zu drücken und zu halten und schließlich die *z*-Taste und alle drei Tasten gleichzeitig wieder los zu lassen.
* `C-M-f` bedeutet, *Control* zu drücken und zu halten, dann *Meta* zu drücken und zu halten, dann *f* zu drücken und alle drei Tasten gleichzeitig wieder los zu lassen.

## Steuerung der Hauptanwendung

* `M-r` - Code ausführen
* `M-s` - Code-Ausführung stoppen
* `M-i` - Hilfesystem ein-/ausschalten
* `M-p` - Einstellungen ein-/ausschalten
* `M-<` - Zum Puffer links wechseln
* `M->` - Zum Puffer rechts wechseln
* `S-M-0` - Switch to buffer 0
* `S-M-1` - Switch to buffer 1
* ...
* `S-M-9` - Switch to buffer 9
* `M-+` - Schrift des Puffers vergrößern
* `M--` - Schrift des Puffers verkleinern


## Auswahl/Kopieren/Einfügen

* `M-a` - Alles auswählen
* `M-c` - Auswahl in den Zwischenspeicher kopieren
* `M-]` - Auswahl in den Zwischenspeicher kopieren
* `M-x` - Auswahl ausschneiden und im Zwischenspeicher ablegen
* `C-]` - Auswahl ausschneiden und im Zwischenspeicher ablegen
* `C-k` - Text ab Cursor bis zum Ende der Zeile ausschneiden
* `M-v` - Inhalt des Zwischenspeichers in den Editor kopieren
* `C-y` - Inhalt des Zwischenspeichers in den Editor kopieren
* `C-SPACE` - Markierung setzen. Textauswahl ab jetzt von hier bis zum Cursor. `C-g` löscht die Markierung.

## Text-Manipulation

* `M-m` - Gesamten Code ausrichten
* `Tab` - Aktuelle Codezeile oder Auswahl ausrichten
* `C-l` - Editor zentrieren
* `M-/` - Aktuelle Codezeile auskommentieren
* `C-t` - Zeichen vertauschen
* `M-u` - Das nächste Wort oder Auswahl in Großbuchstaben wandeln.
* `M-l` - Das nächste Wort oder Auswahl in Kleinbuchstaben wandeln.

## Navigation

* `C-a` - An den Anfang der Zeile springen
* `C-e` - Ans Ende der Zeile springen
* `C-p` - Zur vorherigen Zeile springen
* `C-n` - Zur nächsten Zeile springen
* `C-f` - Ein Zeichen vorwärts
* `C-b` - Ein Zeichen zurück
* `M-f` - Ein Wort nach vorne
* `M-b` - Ein Wort zurück
* `C-M-n` - Zeile oder Auswahl nach unten schieben
* `C-M-p` - Zeile oder Auswahl nach unten schieben
* `S-M-u` - 10 Zeilen nach oben springen
* `S-M-d` - 10 Zeilen nach unten springen
* `M-<` - Zum Anfang des Puffers gehen
* `M->` - Zum Ende des Puffers gehen

## Löschen

* `C-h` - Vorangegangenes Zeichen löschen
* `C-d` - Nächstes Zeichen löschen

## Fortgeschrittene Funktionen des Editors

* `C-i` - Dokumentation für das Wort unter dem Cursor anzeigen
* `M-z` - Rückgängig ("Undo")
* `S-M-z` - Wiederholen ("Redo")
* `C-g` - Escape
* `S-M-f` - Vollbildmodus ein-/ausschalten
* `S-M-b` - Buttons ein-/ausschalten
* `S-M-l` - Anzeige Protokoll-Fenster ein-/aus
* `S-M-m` - Dunkle Benutzeroberfläche ein-/ausschalten
* `S-M-s` - Inhalt des Puffers in Datei speichern
* `S-M-o` - Datei in einen Puffer laden
