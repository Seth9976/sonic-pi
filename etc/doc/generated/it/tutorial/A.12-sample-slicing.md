A.12 Tagliare i campioni

# Tagliare i campioni

Nel terzo episodio di questa serie su Sonic Pi, abbiamo visto come loopare, allungare e filtrare uno dei più famosi break di batteria di tutti i tempi. L'Amen Break. In questo tutorial faremo un passo in avanti per vedere come tagliuzzarlo, riordinare i pezzi e incollare di nuovo tutto insieme in un ordine completamente nuovo. Se ti sembra un po' pazzo, non ti preoccupare; diventerà tutto chiaro non appena capirei come usare un nuovo strumento per i tuoi set di live coding.

## Il suono come dati

Prima di cominciare, prendiamo un momento per analizzare come lavorare con i campioni. Fino ad ora, avrai utilizzato il campionatore di Sonic Pi. Se non l'hai fatto, è il momento di cominciare! Accendi il tuo Rasberry Pi, apri Sonic Pi dal menu Programming e scrivi il seguente codice in un buffer vuoto e poi premi il pulsante Run per sentire un beat di batteria pre-registrato:

```
sample :loop_amen
```

Una registrazione di un suono può essere rappresentato come un flusso di dati: un sacco di numeri compresi tra -1 e 1 che rappresentano i picchi e, più in generale, l'onda sonora. Se suoniamo questi numeri in ordine, otteniamo il suono originale. Ma se decidessimo di suonarli in un ordine diverso per creare un nuovo suono?

Come sono registrati i campioni? È una cosa abbastanza semplice una volta capita la fisica del suono. Quando produci un suono, per esempio colpendo una batteria, il rumore viaggia attraverso l'aria in modo simile al comportamento delle increspature della superficie del lago quando lanci un sasso. Quando queste increspature raggiungono le tue orecchie, il timpano si muove per simpatia e converte questi movimento nel suono che tu ascolti. Se vogliamo registrare e riprodurre un suono, dobbiamo catturare, salvare e riprodurre quelle increspature. Possiamo usare un microfono che agisce come un timpano e si muove avanti e indietro quando il suono lo colpisce. Il microfono converte la sua posizione in un un segnale elettrico che viene misurato molte volte al secondo. Queste misurazioni sono poi rappresentate da una serie di valori compresi tra -1 e 1.

Se dovessimo rappresentare graficamente il suono, risulterebbe un grafico di dati con il tempo sull'asse x e la posizione del microfono/speaker come un valore tra -1 e 1 sull'asse y. Puoi vedere un esempio di questo grafico in cima al diagramma.

## Suonare una parte di un campione

Quindi, come programmiamo Sonic Pi per suonare un campione in ordine differente? Per rispondere a questa domanda, dobbiamo dare un'occhiata ai parametri `start:` e `finish:` di `sample`. Questi valori ci permettono di controllare la posizione iniziale della riproduzione dei numeri che rappresentano i suoni. I valori di questi parametri possono essere rappresentati da numeri compresi tra `0` e `1` dove `0` rappresenta l'inizio del campione e `1` la fine. Per cui, per suonare la prima metà dell'Amen Break, è sufficiente specificare un valore di `finish:` pari a `0.5`:

```
sample :loop_amen, finish: 0.5
```

Possiamo aggiungere un valore `start:` per suonare una porzione del campione:

```
sample :loop_amen, start: 0.25, finish: 0.5
```

Per puro divertimento, puoi avere il parametro `finish:` che sia *prima* di `start:` e il campione suonerà al contrario:

```
sample :loop_amen, start: 0.5, finish: 0.25
```

## Riordinare la riproduzione del campione

Ora che abbiamo capito che i campioni sono un semplice elenco di numeri che possono essere riprodotti in qualsiasi ordine e abbiamo capito come riprodurre una parte specifica; ora possiamo divertirci suonando un campione in un ordine 'sbagliato'.

![Amen Slices](../../../etc/doc/images/tutorial/articles/A.12-sample-slicing/amen_slice.png)

Prendiamo il nostro Amen Break e tagliamolo in 8 parti uguali e poi riordiniamo i vari pezzi. Dai un'occhiata all'immagine; all'inizio A) rappresenta il grafico dei nostri dati originali. Tagliarlo in 8 parti diventa B) - nota come abbiamo assegnato un colore differente a ciascun pezzo per aiutarci a distinguerli. Puoi vedere i valori di inizio e di fine di ciascun pezzo all'inizio. Infine C) è una possibilità di riordinare i pezzi. Poi possiamo suonarlo per creare un nuovo beat. Dai un'occhiata a questo codice:

```
live_loop :beat_slicer do
  slice_idx = rand_i(8)
  slice_size = 0.125
  s = slice_idx * slice_size
  f = s + slice_size
  sample :loop_amen, start: s, finish: f
  sleep sample_duration :loop_amen, start: s, finish: f
end
```

1. Abbiamo scelto un pezzo in modo casuale prendendo un numero compreso tra 0 e 7 (ricordati che iniziamo a contare da 0). Sonic Pi ha una funzione che ci viene in aiuto: `rand_i(8)`. Dopodiché salviamo il valore di indice nella variabile `slice_idx`.
2. Definiamo la nostra `slice_size` che è 1/8 o 0.125. Il parametro `slice_size` è necessario per noi per convertire `slice_idx` in un valore compreso tra 0 e 1 per poterlo usare come parametro di `start:`.
3. Calcoliamo la posizione iniziale `s` moltiplicando l'indice `slice_idx` per la dimensione `slice_size`.
4. Calcoliamo la posizione finale `f` aggiungendo la dimensione della sottosequenza `slice_size` alla posizione iniziale `s`.
5. Ora possiamo riprodurre i pezzi del campione agganciando i valori `s` e `f` dentro i parametri `start:` e `finish:` di `sample`.
6. Prima di iniziare a suonare il pezzo successivo, dobbiamo sapere quanto lungo deve essere il valore di `sleep` che dovrebbe essere uguale alla durata del pezzo del campione. Per fortuna Sonic Pi ha la funzione `sample_duration` che accetta gli stessi parametri di `sample` e restituisce il valore della durata. Per cui, passando il valore `sample_duration` ai parametri `start:` e `finish:`, possiamo conoscere la durata di un singolo pezzo.
7. Inseriamo tutto questo codice in un `live_loop` per poter continuare a scegliere di suonare un pezzo casualmente.


## Mettiamo tutto insieme

Combiniamo tutto quello che abbiamo visto fino ad ora in un ultimo esempio che rappresenti come usare un approccio simile per combinare pezzi di campione con una nuova linea di basso per creare l'inizio di una traccia. Ora è il tuo turno, prendi il codice qui sotto come punto di partenza e vedi se puoi trovare la tua strada per creare qualcosa di nuovo...

```
live_loop :sliced_amen do
  n = 8
  s =  line(0, 1, steps: n).choose
  f = s + (1.0 / n)
  sample :loop_amen, beat_stretch: 2, start: s, finish: f
  sleep 2.0  / n
end
live_loop :acid_bass do
  with_fx :reverb, room: 1, reps: 32, amp: 0.6 do
    tick
    n = (octs :e0, 3).look - (knit 0, 3 * 8, -4, 3 * 8).look
    co = rrand(70, 110)
    synth :beep, note: n + 36, release: 0.1, wave: 0, cutoff: co
    synth :tb303, note: n, release: 0.2, wave: 0, cutoff: co
    sleep (ring 0.125, 0.25).look
  end
end
```
