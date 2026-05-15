A.9 Randomizzazione

# Scivolare su correnti casuali

Tornando indietro all'episodio 4 di questo tutorial, avevamo dato un breve sguardo alla randomizzazione mentre programmavamo alcuni riff di synth. Considerando che la randomizzazione è una parte importante della mia pratica musicale live code DJ, ho pensato che fosse utile di trattare i fondamentali di questo aspetto più nel dettaglio. Quindi, mettiti comodo e partiamo sulle strade della randomizzazione!

## Il caso non esiste

La prima cosa da imparare, e che potrebbe sorprenderti quando suoni le funzioni random di Sonic PI, è che in realtà esse non sono davvero casuali. Che significa questo? Proviamo a fare un paio di prove. Primo: prova a pensare un numero tra 0 e 1. Tienitelo in mente e non dirmelo. Provo a indovinare...Era '0.321567'? No? Bah, non sono molto bravo allora. Fammi fare un altro tentativo, ma stavolta fai scegliere il numero a Sonic Pi. Avvia il programma e chiedigli un numero casuale; ma anche stavolta non dirmelo:

```
print rand (stampa i numeri casuali)
```

Quindi riproviamoci...era `0.75006103515625`? Sì! Riesco a vedere il tuo scetticismo. Forse era solo fortuna. Riproviamo. Premi di nuovo Run e vediamo che cosa esce...Cosa, `0.75006103515625` di nuovo? Questo non può essere sicuramente casuale! Sì, hai ragione, non lo è.

Che sta succedendo? La parola usata in informatica è determinismo. Questo significa che niente è casuale e tutto è destinato a esserlo. La tua versione di Sonic Pi è destinata sempre a restituire il numero `0.75006103515625` se esegui il programma scritto sopra. Questo potrebbe sembrarti piuttosto inutile, ma ti assicuro che invece è uno degli aspetti più utili di Sonic Pi. Se ti applichi un po' imparerai come rapportarti alla natura deterministica della randomizzazione di Sonic Pi e capirai che questo aspetto è fondamentale per le tue composizioni e le tue performance da DJ.

## Una melodia casuale

Quando Sonic Pi si avvia, in realtà carica nella memoria una sequenza di 441000 valori casuali già generati. Quando richiami una funzione che genera casualità, come 'rand' o 'rrand', questo flusso casuale è usato per generare il tuo risultato. Ogni richiamo di una funzione di questo tipo prende i valori da questo flusso. Quindi la decima chiamata di una funzione random userà il decimo valore di quel flusso. Inoltre, ogni volta che premi Run, il flusso riparte da zero. Questo è il motivo per cui sono riuscito a predire il risultato prima ed è anche il motivo per cui una melodia 'casuale' è sempre uguale ogni volta. Ogni versione di Sonic Pi usa lo stesso flusso di valori casuali. Questo è molto importante quando cominciamo a condividere i nostri brani musicali tra noi.

Usiamo quanto appena imparato per generare una melodia casuale ripetibile:

```
8.times do
 play rrand_i(50, 95)
 sleep 0.125
end
```

Digita il codice sopra in una finestra vuota e premi Run. Ascolterai una melodia di note 'casuali' incluse tra 50 e 95. Quando è terminata, premi di nuovo Run e sentirai esattamente la stessa melodia.

## Funzioni utili di randomizzazione

Sonic Pi viene fornito con una serie di funzioni utili per lavorare con il flusso di dati casuali. Eccoti una lista di alcune delle migliori:

* 'rand' - Semplicemente restituisce il prossimo valore del flusso di numeri casuali
* 'rrand' - Restituisce un valore casuale all'interno di due valori predeterminati
* 'rrand_i' - Restituisce un numero intero casuale all'interno di un intervallo predeterminato
* 'one_in' - è vero o falso secondo una casualità predeterminata
* 'dice' - imita il comportamento di un dado e quindi restituisce valori casuali tra 1 e 6
* 'choose' - sceglie valori casuali da una lista

Dai un'occhiata alla loro documentazione nella sezione Help se vuoi maggiori informazioni o degli esempi.

## Azzerare il flusso

L'abilità di ripetere una sequenza di note predeterminate è essenziale per riprodurre un riff mentre suoni. Tuttavia, quello che senti potrebbe non essere esattamente il riff a cui avevi pensato. Non sarebbe bello poter provare alcuni differenti riff e scegliere quello preferito? Qui è dove la magia ha inizio.

Possiamo regolare il flusso usando la funzione 'use_random_seed'. In informatica, un random seed, ovvero un generatore di numeri casuali, ha un punto di inizio dal quale il flusso di valori causali parte. Prova quanto segue:

```
use_random_seed 0
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Bene! Abbiamo ottenuto le prime tre note della nostra melodia casuale: '84', '83' e '71. Tuttavia, se vogliamo possiamo cambiare questa serie. Prova a eseguire questo:

```
use_random_seed 1
3.times do
  play rrand_i(50, 95)
  sleep 0.125
end
```

Interessante, abbiamo ottenuto '83', '71' e '61'. Forse noterai che le prime due cifre di questa serie sono le stesse delle ultime due appartenenti alla serie precedente. E non è una coincidenza.

Ricordati che il flusso di dati casuali è semplicemente una lista gigante di valori già predeterminati. Eseguendo 'use_random_seed' scegliamo da dove partire. Un'altra immagine che può renderti l'idea è un enorme mazzo di carte già mischiato. Usando questa funzione, noi scegliamo dove tagliare il mazzo. La cosa eccitante di questa funzione è proprio la sua abilità di saltare al punto del flusso che vogliamo, dandoci infinite possibilità musicali.

Riguardiamo brevemente la nostra melodia casuale di 8 note implementando questa funzione. Prima però inserisci il codice all'interno di un live loop, così possiamo effettuare modifiche mentre la melodia continua a suonare:

```
live_loop :random_riff do    
  use_random_seed 0
  8.times do
    play rrand_i(50, 95), release: 0.1
    sleep 0.125
  end
end
```
  
Adesso, mentre sta ancora suonando, cambia il valore della funzione da '0' a qualcos'altro. Prova '100' oppire '999'. Prova anche valori a tua scelta, sperimenta e divertiti. Valuta qual'è il riff che ti piace di più.

## Mettiamo tutto insieme

Il tutorial di questo mese è stato un bel tuffo nelle funzioni di casualità utilizzabili in Sonic Pi. Spero di essere riuscito a darvi un'idea di come funziona e di aver suggerito un punto di partenza per usare la casualità in modo affidabile per creare pattern ripetitivi nella tua musica. È importante ribadire che puoi usare questa casualità ripetute *dovunque* tu voglia. Per esempio lo puoi fare per il volume delle note, il tempo di un ritmo, la quantità di riverbero, il synth corrente, il mix di un effetto ecc... In futuro vedremo alcune di queste applicazioni in dettaglio ma, per oggi, vi lascio con un piccolo esempio.

Scrivi il seguente codice in un buffer vuoto, premi Run e prova a cambiare il valore di seed. Premi Run di nuovo (mentre sta andando) e esplora le differenze sonore, di ritmo e di melodia che riesci a fare. Quando ne trovi una bella, ricordati del numero di seed per poterla risuonare. Infine, quando hai trovato un secondo valore di seed che produce risultati interessanti, fai una performance per i tuoi amici cambiando semplicemente tra un valore di seed e l'altro.

```
live_loop :random_riff do
  use_random_seed 10300
  use_synth :prophet
  s = [0.125, 0.25, 0.5].choose
  8.times do
    r = [0.125, 0.25, 1, 2].choose
    n = (scale :e3, :minor).choose
    co = rrand(30, 100)
    play n, release: r, cutoff: co
    sleep s
  end
end
live_loop :drums do
  use_random_seed 2001
  16.times do
    r = rrand(0.5, 10)
    sample :drum_bass_hard, rate: r, amp: rand
    sleep 0.125
  end
end
```
