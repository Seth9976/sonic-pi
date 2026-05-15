10.2 Tabel de combinatii de taste

# Tabel de combinatii de taste

The following is a summary of the main Emacs Live shortcuts available within Sonic Pi. Please see Section B.1 for motivation and background.

## Conventii

In aceasta lista folosim urmatoarele conventii (unde *Meta* este *Alt* pentru Windows/Linux sau *Cmd* pentru Mac):

* 'C-a' inseamna sa tii apasata tasta *Control* apoi sa apesi tasta *a* astfel incat sa fie ambele apasate simultan, apoi sa le eliberezi.
* 'M-r' inseamna sa tii apasata tasta *Meta* apoi sa apesi tasta *r* astfel incat sa fie ambele apasate simultan, apoi sa le eliberezi.
* 'S-M-z' inseamna sa tii apasata tasta *Shift*, apoi tasta *Meta*, apoi sa apesi tasta *z* astfel incat sa fie toate apasate simultan, apoi sa le eliberezi.
* 'C-M-f' inseamna sa tii apasata tasta *Control*, apoi tasta *Meta*, apoi sa apesi tasta *f* astfel incat sa fie toate apasate simultan, apoi sa le eliberezi.

## Controlul aplicatiei principale

* `M-r` - Executa codul (Run)
* `M-s` - Opreste codul (Stop)
* 'M-i' - Activeaza/dezactiveaza sistemul de ajutor (help)
* `M-p` - Activeaza/dezactiveaza optiunile
* `M-{` - Comuta la buffer-ul din stanga
* `M-}` - Comuta la buffer-ul din dreapta
* `S-M-0` - Switch to buffer 0
* `S-M-1` - Switch to buffer 1
* ...
* `S-M-9` - Switch to buffer 9
* `M-+` - Creste dimensiunea textului in buffer-ul curent
* `M--` - Micsoreaza dimensiunea textului in buffer-ul curent


## Selectie/Copy/Paste

* 'M-a' - Selecteaza tot
* `M-c` - Copiaza selectia in memorie
* `M-]` - Copiaza selectia in memorie
* `M-x` - Muta selectia in memorie (cut)
* `C-]` - Muta selectia in memorie (cut)
* `C-k` - Muta in memorie (cut) totul pana la sfarsitul liniei
* `M-v` - Aduce din memorie (paste) in editor
* `C-y` - Aduce din memorie (paste) in editor
* 'C-SPATIU' - Trece in mod evidentiere. Comenzile de navigare vor actiona in zona evidentiata. Foloseste 'C-g' ca sa revii in modul normal.

## Manipularea textului

* 'M-m' - Aliniaza tot textul
* `Tab` - Aliniaza linia sau selectia curenta (sau trece in modul auto-completare)
* `C-l` - Centreaza editorul
* `M-/` - Transforma linia sau selectia curenta in comentariu/revine la text normal
* `C-t` - Inverseaza ordinea caracterelor
* `M-u` - Converteste urmatorul cuvant (sau selectie) la majuscule.
* `M-l` - Converteste urmatorul cuvant (sau selectie) la litere mici.

## Navigarea

* `C-a` - Merge la inceputul liniei
* `C-e` - Merge la sfarsitul liniei
* `C-p` - Merge la linia precedenta
* `C-n` - Merge la linia urmatoare
* `C-f` - Merge inainte un caracter
* `C-b` - Merge inapoi un caracter
* `M-f` - Merge inainte un cuvant
* `M-b` - Merge inapoi un cuvant
* `C-M-n` - Muta linia sau selectia in jos
* `C-M-p` -  Muta linia sau selectia in sus
* `S-M-u` - Merge in sus 10 linii
* `S-M-d` - Merge in jos 10 linii
* `M-<` - Merge la inceputul buffer-ului
* `M->` - Merge la sfarsitul buffer-ului

## Stergerea

* `C-h` - Sterge caracterul precedent
* `C-d` - Sterge caracterul urmator

## Functii avansate ale editorului

* `C-i` - Arata documentatia pentru cuvantul de la pozitia cursorului
* `M-z` - Undo (anuleaza ultima modificare)
* `S-M-z` - Redo (reface ultima modificare)
* `C-g` - Escape (Revine la modul anterior)
* `S-M-f` - Activeaza/dezactiveaza modul ecran complet
* `S-M-b` - Afiseaza/ascunde butoanele
* `S-M-l` - Afiseaza/ascunde jurnalul (log)
* `S-M-m` - Comuta intre modurile luminos/intunecat
* `S-M-s` - Salveaza continutul bufferului intr-un fisier
* `S-M-o` - Incarca dintr-un fisier continutul bufferului
