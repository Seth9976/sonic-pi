A.3 Beat codificati

# Beat codificati

Tra gli sviluppi tecnologici che hanno avuto maggiore impatto nel mondo della musica moderna, c'è sicuramente l'invenzione dei campionatori. Inizialmente i campionatori erano degli scatolotti in grado di registrare e mantenere al proprio interno un qualsiasi suono, in maniera da poterlo manipolare e riprodurre in tanti modi diversi. Per esempio, potevi prendere una vecchia canzone, individuare un assolo di batteria, registrarlo all'interno del campionatore, e successivamente riprodurlo in ripetizione continua ad una velocità dimezzata. Questo è esattamente il modo con cui sono stati creati i primi beat hip hop e oggi è praticamente impossibile travare nella musica elettronica una canzone che non contenga in qualche modo dei “campioni”. L'utilizzo dei campioni è l'ideale per introdurre in maniera semplice e veloce interessanti elementi nella tua esibizione di live coding.

Ma quindi dove lo troviamo un campionatore? Beh, in realtà ne abbiamo già uno. E' nel nostro Raspberry Pi! Nel cuore di Sonic Pi è integrato un campionatore molto potente: proviamo a giocarci un po'!

## L'Amen Break

Uno dei campioni di batteria più conosciuti e maggiormente riconoscibili è L'Amen Break. L'Amen Break è contenuto in una canzone del 1969 intitolata "Amen Brother" eseguita dalla band americana The Winstons. "Break" in Inglese significa pausa e nella musica rappresenta un interludio strumentale che collega due parti di una canzone. Questo particolare Break fu scoperto all'inizio negli anni 80 da dei musicisti hip hop e da lì utilizzato in una grande varietà di generi musicali come drum and bass, breakbeat, hardcore techno and breakcore.

Sarai contento di sapere che questo particolare campione è già incluso in Sonic Pi. Ripulisci un buffer e scrivici dentro il seguente codice:

```
sample :loop_amen
```

Premi *Run* e bum! Stai ascoltando uno dei più influenti campioni di batteria della storia della musica dance! Tuttavia, questo campione non è famoso per essere suonato singolarmente, bensì per essere riprodotto all'interno di un loop.


## Stretchare il beat

Eseguiamo l'Amen Break in loop facendoci aiutare dal nostro vecchio amico `live_loop`, già introdotto in questo tutorial:

```
live_loop :amen_break do
  sample :loop_amen
  sleep 2
end
```

Ok, è in loop, ma c'è una fastidiosa pausa ad ogni ripetizione. Questo è perché abbiamo chiesto uno sleep pari a `2` battute e con una velocità predefinita di 60 BPM, il nostro campione `:loop_amen` dura soltanto `1.753` battute. Quindi abbiamo un silenzio di `2 - 1.753 = 0.247` battute. Anche se è breve, risulta ben evidente.

Per risolvere questo problema, possiamo usare `beat_stretch:` chiedendo a Sonic Pi di "streatchare" (stiracchiare) il campione, in modo da farlo durare uno specifico numero di battute.

Le funzioni `sample` e `synth` ti danno la possibilità di avere un grande controllo attraverso i parametri opzionali `amp:`, `cutoff:` e `release:`. Tuttavia, il termine "parametri opzionali" è lungo e complicato da dire, quindi utilizzeremo la parola *opts*.

```
live_loop :amen_break do
  sample :loop_amen, beat_stretch: 2
  sleep 2
end  
```

Adesso si che si balla! Anche se, forse vorremmo rallentare o accelerare il ritmo per adattarlo al nostro umore.

## Giocare con il tempo

Bene, quindi che dobbiamo fare se vogliamo cambiare stile e passare all'hip hop old school o al breakcore? Un modo semplice è di giocare con il tempo, o meglio incasinare il tempo. Con Sonic Pi è facilissimo, basta infilare un 'use_bpm' dentro il tuo live loop:

```
live_loop :amen_break do
  use_bpm 30
  sample :loop_amen, beat_stretch: 2
  sleep 2
end 
```

Mentre stai rappando sopra questi ritmi lenti, fai caso a come il programma rimanga fermo per 2 battiti (sleep 2) e i nostri bpm sono a 30. Quindi tutto è a tempo. L'opzione 'beat_stretch' funziona con i bpm di riferimento per fare in modo che tutto torni.

Quindi, qui sta la parte divertente. Mentre il loop è ancora in azione, cambia il numero '30' con '50' dentro 'use_bpm 30'. Wow, tutto va più veloce e comunque rimane *a tempo*! Prova ad andare ancora più veloce, inserendo 80 o 120 e ora fai impazzire il computer e spacca a 200!


## Filtraggio

Adesso che abbiamo imparato a mettere i campioni nel live loop, diamo un'occhiata ad alcune delle opzioni più divertenti del synth 'sample'. Il primo è 'cutoff:', che controlla il filtro di taglio del campionatore. Di norma è disabilitato ma puoi facilmente attivarlo:

```
live_loop :amen_break do
  use_bpm 50
  sample :loop_amen, beat_stretch: 2, cutoff: 70
  sleep 2
end  
```

Vai avanti e cambia il 'cutoff:'. Per esempio, aumentalo a 100, premi *Run* e aspetta che il loop torni all'inizio per ascoltare il risultato sonoro. I valori più bassi, tipo 50, suonano più bassi e caldi mentre valori alti tipo 100 o 120 sono più secchi. Ciò accade perché l'opzione 'cutoff:' taglia le frequenze alte nello stesso modo in cui una falciatrice taglia l'erba. Il valore di 'cutoff:' è come regolare l'altezza della falciatrice e determina quanto l'erba deve essere tagliata.


## Affettare

Un altro strumento divertente è l'effetto slicer. Questo affetterà (slice in inglese) il suono. Avvolgi la linea di 'sample' dentro il codice di effetto, così:

```
live_loop :amen_break do
  use_bpm 50
  with_fx :slicer, phase: 0.25, wave: 0, mix: 1 do
    sample :loop_amen, beat_stretch: 2, cutoff: 100
  end
  sleep 2
end
```

Fai caso a come il suono salta sù e giù (se vuoi sentire il suono originale, senza effetti, cambia il valore di 'mix:' a 0). Adesso, proviamo a giocare con i valori di 'phase:'. Questa è la frequenza (in battiti) dell'effetto di affettamento. Un valore piccolo, tipo '0.125', affetterà più velocemente e uno più grande, tipo '0.5', più lentamente. Senti come il dimezzamento o il raddoppiamento dei valori di 'phase:' tendono sempre a suonare piacevoli. Infine, cambia il valore di 'wave:' a 0, 1 o 2 e ascolta la differenza. I diversi numeri rappresentato le differenti forme d'onda. 0 è un'onda a dente di sega (attacco veloce e rilascio lento), 1 è un'onda quadrata ( attacco e rilascio veloce), 2 infine è un'onda triangolare (attacco e rilascio lento).


## Mettiamo tutto insieme

Infine, andiamo indietro nel tempo al primo periodo drum and bass di Bristol con questo esempio. Non ti preoccupare troppo di cosa significa questa frase. Digita il codice, premi Run e comincia a fare live coding cambiando i valori delle opzioni e osserva i risultati. Mi raccomando, condividi quello che hai ottenuto. Ci vediamo la prossima volta...

```
use_bpm 100
live_loop :amen_break do
  p = [0.125, 0.25, 0.5].choose
  with_fx :slicer, phase: p, wave: 0, mix: rrand(0.7, 1) do
    r = [1, 1, 1, -1].choose
    sample :loop_amen, beat_stretch: 2, rate: r, amp: 2
  end
  sleep 2
end
live_loop :bass_drum do
  sample :bd_haus, cutoff: 70, amp: 1.5
  sleep 0.5
end
live_loop :landing do
  bass_line = (knit :e1, 3, [:c1, :c2].choose, 1)
  with_fx :slicer, phase: [0.25, 0.5].choose, invert_wave: 1, wave: 0 do
    s = synth :square, note: bass_line.tick, sustain: 4, cutoff: 60
    control s, cutoff_slide: 4, cutoff: 120
  end
  sleep 4
end
```
