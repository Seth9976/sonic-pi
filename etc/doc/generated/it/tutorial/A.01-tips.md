A.1 Consigli per Sonic Pi

# I cinque consigli migliori

## 1. Non ci sono errori

La lezione più importante da imparare con Sonic Pi è che non ci sono errori. Il modo migliore di imparare è provando. Prova un sacco di cose, smettila di preoccuparti se il tuo codice suona bene o no e prova diversi synth, note, effetti e parametri. Scoprirai un sacco di cose che ti faranno divertire perché suonano in modo orribile e alcune gemme preziose che suoneranno incredibilmente. Semplicemente elimina le cose che non ti piacciono e tieni quelle che ti piacciono. Più 'errori' ti permetti di fare, più velocemente imparerai.


## 2. Usa gli effetti

Scommetto che hai imparato a utilizzare le funzioni di base di Sonic Pi come `sample` e `play`. Cosa c'è dopo? Lo sapevi che Sonic Pi ha al suo interno più di 27 effetti per cambiare il suono del tuo codice? Gli effetti sono come i filtri nei programmi di grafica solo che invece di sfocare l'immagine o trasformarla in bianco e nero aggiungono riverbero, distorsione, echo. Pensali come se stessi collegando un pedale da chitarra all'amplificatore. Per fortuna Sonic Pi rende l'utilizzo degli FX super semplice e non richiede cavi! Tutto quello che devi fare è prendere una sezione di codice e racchiuderla dentro un effetto. Proviamo con un esempio:

```
sample :loop_garzul
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Se vuoi aggiungere un effetto al campione `:loop_garzul`, metti tutto dentro il blocco `with_fx`:

```
with_fx :flanger do
  sample :loop_garzul
end
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Ora, se vuoi aggiungere un effetto alla cassa della batteria, inserisci anche quella dentro `with_fx`:

```
with_fx :flanger do
  sample :loop_garzul
end
with_fx :echo do
  16.times do
    sample :bd_haus
    sleep 0.5
  end
end
```

Ricorda: puoi inserire *qualsiasi* codice dentro `with_fx` e ogni suono creato passerà attraverso quell'effetto.


## 3. Controllare i sintetizzatori

Per scoprire i tuoi suoni, avrai presto bisogno di modificar e controllare i tuoi synth e gli effetti. Per esempio, magari vuoi cambiare la lunghezza di una nota, aggiungere più riverbero, cambiare il tempo tra le ripetizioni dell'eco. Sonic Pi ti permette di avere un grande livello di controllo attraverso i parametri opzionali. Vediamo velocemente come fare. Copia il seguente codice e premi Run:

```
sample :guit_em9
```

Ooh, un bellissimo suono di chitarra. Ora cominciamo a giocarci. Perché non cambiare la velocità?

```
sample :guit_em9, rate: 0.5
```

Hey, cos'è quel `rate: 0.5` che ho appena aggiunto alla fine? È un parametro. Tutti i synth e gli effetti in Sonic Pi li supportano e ce ne sono un sacco con cui giocare. Sono disponibili anche per gli effetti; prova questo:

```
with_fx :flanger, feedback: 0.6 do
  sample :guit_em9
end
```

Ora, prova ad aumentare il feedback a 1 per sentire dei suoni assurdi. Leggi la documentazione per tutti i dettagli sui parametri che puoi utilizzare.


## 5. Programma live

Il modo migliore per sperimentare velocemente con Sonic Pi è attraverso il live code. Questo ti permette di partire con del codice e continuare a cambiarlo mentre continua a funzionare. Per esempio, se non sai cosa fa il parametro di cutoff a un campione, prova a usarlo. Copia questo codice in un workspace:

```
live_loop :experiment do
  sample :loop_amen, cutoff: 70
  sleep 1.75
end
```

Ora previ run e sentirai un suono di batteria soffocato. Ora cambia il valore di `cutoff:` a `80` e premi di nuovo run. Riesci a sentire la differenza? Ora prova `90`, `100`, `110`...

Una volta che capisci come usare `live_loop` non tornerai più indietro. Ogni volta che faccio una performance, mi baso completamente su `live_loop` tanto quanto un batterista si affida alle sue bacchette. Per maggiori informazioni sul live coding, guarda la sezione 9 del tutorial all'interno di Sonic Pi.

## 5. Naviga le acqua della casualità

Una delle cose che amo fare con Sonic Pi è fare in modo che componga al posto mio. Un ottimo modo per farlo è utilizzare la casualità. Può sembrare complicato ma non lo è per niente. Dai un'occhiata a questo codice:

```
live_loop :rand_surfer do
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Ora, quando suoni questo, sentirai un flusso costante di nome della scala `:e2 :minor_pentatonic` suonate con un synth `:dsaw`. Vi sento gridare: "Aspetta un momento, quella non è una melodia". Beh, questa è la prima parte del mio trucco magico. Ogni volta che riproduciamo dall'inizio `live_loop` possiamo dire a Sonic Pi di resettare il flusso casuale. È come andare indietro nel tempo. Prova questo, aggiungi la linea `use_random_seed 1` al `live_loop`:

```
live_loop :rand_surfer do
  use_random_seed 1
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Ogni volta che il `live_loop` ricomincerà, i numeri random saranno resettati. Questo significa che sceglierà sempre le stesse 16 note ogni volta. Voilà, una melodia. Ora, questa è la parte interessante. Cambia il valore del seme da `1` a, diciamo, `4923`. Wow! Un'altra melodia! Semplicemente cambiando un numero (quello del seme), puoi esplorare tutte le combinazioni di melodie che vuoi. Questa è magia.
