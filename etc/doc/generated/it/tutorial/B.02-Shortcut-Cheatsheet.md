10.2 Riassunto scorciatoie

# Riassunto scorciatoie

The following is a summary of the main Emacs Live shortcuts available within Sonic Pi. Please see Section B.1 for motivation and background.

## Convenzioni

In questa lista vengono usate le seguenti convenzioni (dove *Meta* è *Alt* su Windows/Linux o *Cmd* su Mac):

* `C-a` significa tieni premuto *Control* e il tasto *a* contemporaneamente.
* `M-r` significa tieni premuti il tasto *Meta* e poi premi *r*.
* `S-M-z` vuol dire premi contemporaneamente *Shift*, *Meta* e il tasto *z*.
* `C-M-f` significa premi *Control*, *Meta* e *f* contemporaneamente.

## Controllo dell'applicazione principale

* `M-r` - Avvia il codice
* `M-s` - Ferma il codice
* `M-i` - Apri/Chiudi il sistema di aiuto
* `M-p` - Apri/Chiudi le preferenze
* `M-{` - Sposta il buffer a sinistra
* `M-}` - Sposta il buffer a destra
* `S-M-0` - Passa al buffer 0
* `S-M-1` - Passa al buffer 1
* ...
* `S-M-9` - Passa al buffer 9
* `M-+` - Ingrandisci la dimensione carattere del buffer corrente
* `M--` - Diminuisci la grandezza dei caratteri nel buffer corrente


## Selezione/Copia/Incolla

* `M-a` - Seleziona tutto
* `M-c` - Copia
* `M-]` - Copia
* `M-x` - Taglia
* `C-]` - Taglia
* `C-k` - Taglia fino alla fine della riga
* `M-v` - Incolla
* `C-y` - Incolla
* `C-SPACE` - Imposta un segnalibro. La navigazione controllerà la regione selezionata. Premi `C-g` per uscire.

## Controllo del testo

* `M-m` - Allinea il testo
* `Tab` - Allinea la linea corrente (oppure seleziona l'autocompletamento)
* `C-l` - Centra l'editor
* `M-/` - Commenta/Scommenta la linea corrente
* `C-t` - Trasponi/scambia caratteri
* `M-u` - Converti la parola successiva in maiuscolo.
* `M-l` - Converti la parola successiva in minuscolo.

## Navigazione

* `C-a` - Spostati all'inizio della riga
* `C-e` - Spostati alla fine della riga
* `C-p` - Spostati alla linea precedente
* `C-n` - Spostati su quella successiva
* `C-f` - Spostati avanti di un carattere
* `C-b` - Spostati indietro di un carattere
* `M-f` - Spostati avanti di una parola
* `M-b` - Spostati indietro di una parola
* `C-M-n` - Sposta linea o selezione in basso
* `C-M-p` - Sposta linea o selezione in alto
* `S-M-u` - Spostati in su di 10 righe
* `S-M-d` - Spostati in giù di 10 righe
* `M-<` - Spostati all'inizio del buffer
* `M->` - Spostati alla fine del buffer

## Cancellazione

* `C-h` - Cancella il carattere precedente
* `C-d` - Cancella il carattere successivo

## Caratteristiche avanzate dell'Editor

* 'C-i' - Mostra i documenti collegati alla parola dove si trova il cursore
* 'M-z' - Annulla
* 'S-M-z' - Torna indietro dopo l'annullamento
* 'C-g' - Esci
* 'S-M-f - Avvia la modalità a schermo intero
* 'S-M-b' - Attiva la visibilità dei bottoni
* 'S-M-l' - Rende visibile la finestra di log
* 'S-M-m' - Passa dall'interfaccia scura a quella chiara e viceversa
* `S-M-s` - Salva il contenuto del buffer in un file
* `S-M-o` - Carica il contenuto del buffer da un file
