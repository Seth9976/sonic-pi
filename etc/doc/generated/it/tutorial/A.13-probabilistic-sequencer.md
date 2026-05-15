A.13 Programma un Sequencer Probabilistico

# Programma un Sequencer Probabilistico

In un precedente episodio della serie su Sonic Pi abbiamo scoperto il potere della randomizzazione, utile a introdurre varietà e sorpresa durante le nostre sessioni di live coding. Per esempio, abbiamo scelto a caso delle note da una scala per comporre melodie infinite. Oggi impareremo una nuova tecnica che usa la randomizzazione per i beat del ritmo!

## Probabilità

Prima di cominciare a creare ritmi e synth, facciamo una breve panoramica su concetti base di probabilità. Detta così può sembrare complicato e spaventoso ma, in realtà, è tanto semplice quanto lanciare un dado. Quando lanci un normale dado a 6 facce in un gioco da tavolo cosa succede? Al primo tentativo i numeri 1, 2, 3, 4, 5 e 6 avranno tutti la stessa probabilità di uscire. Di fatto, essendo un dado a 6 facce, in media (se lo lanci molte volte) ti uscirà il numero 1 ogni 6 lanci. Questo significa avere una possibilità di 1 su 6 che esca 1. Possiamo simulare lo stesso comportamento in Sonic Pi utilizzando la funzione `dice`. Lanciamone uno 8 volte:

```
8.times do
  puts dice
  sleep 1
end
```

Nota come il log mostri valori tra 1 e 6 esattamente come se avessimo lanciato un vero dado.

## Beat casuali

Ora, immagina di avere una batteria e che ogni volta che stai per colpirla decidi di lanciare un dado. Se esce 1, colpisci la batteria altrimenti no. Ora abbiamo una drum machine probabilistica che funziona con una probabilità di 1/6. Proviamo a sentire il risultato:

```
live_loop :random_beat do
  sample :drum_snare_hard if dice == 1
  sleep 0.125
end
```


Guardiamo ogni riga di codice per essere sicuri che sia tutto chiaro. Per prima cosa creiamo un nuovo `live_loop` chiamato `:random_beat` che si ripeterà continuamente tra le righe `do` e `end`. La prima di queste righe è una chiamata a `sample` che riprodurrà un suono preregistrato (in questo caso il suono `:drum_snare_hard`). Tuttavia questa riga ha una condizione `if`alla fine. Questo significa che la linea verrà eseguita se la condizione che è scritta alla destra di `if` risulterà `true` (vero). In questo caso: `dice == 1`. Questa è una chiamata alla funzione `dice` che, come abbiamo visto, restituisce un valore tra 1 e 6. Usiamo l'operatore `==` per verificare se il valore è uguale a `1`. Se è così, la condizione restituisce `true` e viene riprodotto il suono di rullante, se il numero è diverso da `1` restituisce `false` e il rullante viene saltato. La seconda linea aspetta semplicemente che passino `0.125` secondi prima di lanciare nuovamente il dado.

## Cambiare le probabilità

Quelli di voi che hanno fatto giochi di ruolo sanno che esistono dadi di diverse forme e con differenti numeri di facce. Per esempio esiste il dato a forma di tetraedro che ha solo 4 lati oppure l'icosaedro che, invece, ha 20 facce. Il numero di facce determina il cambiamento nella probabilità che esca 1. Minore è il numero di facce, più probabilità hai che esca 1. Per esempio in un dato a 4 facce, c'è 1 possibilità su 4 mentre in quello da 20 è 1 su 20. Per fortuna Sonic Pi ha la funzione `one_in` che descrive esattamente questo. Proviamo:

```
live_loop :different_probabilities do
  sample :drum_snare_hard if one_in(6)
  sleep 0.125
end
```

Facendo partire il live loop qui sopra, sentirai un ritmo casuale familiare. Ora: non fermare la musa ma cambia il `6` con un valore diverso come `2` o `20` e premi nuovamente `Run`. Nota come più basso è il numero più frequentemente senti il suono di rullante mentre più è alto il numero minori sono le probabilità che venga suonato. Stai creando musica con le probabilità!

## Combinare le probabilità

Le cose si fanno davvero interessanti quando diversi campioni vengono avviati con diverse probabilità. Per esempio:

```
live_loop :multi_beat do
  sample :elec_hi_snare if one_in(6)
  sample :drum_cymbal_closed if one_in(2)
  sample :drum_cymbal_pedal if one_in(3)
  sample :bd_haus if one_in(4)
  sleep 0.125
end
```

Prova a far partire il codice qui sopra cambiando ancora una volta le probabilità per modificare il ritmo. Puoi provare anche a cambiare il sample per cambiare completamente il sentimento. Modifica `:drum_cymbal_closed` in `:bass_hit_c` per avere del basso extra!


## Ritmi ripetibili

Infine, possiamo usare il caro vecchio `use_random_seed` per resettare il flusso casuale dopo 8 iterazioni per creare un beat regolare. Scrivi il seguente codice per sentire un ritmo più regolare e ripetitivo. Una volta ascoltato, prova a cambiare il valore di seed da `1000` a un altro numero. Nota come numeri differenti generato beat diversi.

```
live_loop :multi_beat do
  use_random_seed 1000
  8.times do
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus if one_in(4)
    sleep 0.125
  end
end
```

Solitamente, con questo tipo struttura bisogna ricordarsi quali semi suonano bene e prenderne nota. In questo modo, posso facilmente riproporre dei ritmi che ho già creato in concerti futuri.

## Mettiamo tutto insieme

Infine possiamo aggiungere un suono di basso casuale per dare un po' di contenuto melodico. Ricorda che possiamo usare il sistema di sequenze probabilistiche che abbiamo appena scoperto non solo per i sample ma anche per i sintetizzatori. Non lasciarlo così com'è, cambia i numeri e crea la tua traccia con la potenza delle probabilità!

```
live_loop :multi_beat do
  use_random_seed 2000
  8.times do
    c = rrand(70, 130)
    n = (scale :e1, :minor_pentatonic).take(3).choose
    synth :tb303, note: n, release: 0.1, cutoff: c if rand < 0.9
    sample :elec_hi_snare if one_in(6)
    sample :drum_cymbal_closed if one_in(2)
    sample :drum_cymbal_pedal if one_in(3)
    sample :bd_haus, amp: 1.5 if one_in(4)
    sleep 0.125
  end
end
```
