4 Randomizzazione

# Randomizzazione

Un modo molto efficace per rendere interessante la tua musica è utilizzare numeri casuali. Sonic Pi ha delle funzioni che ti permettono di introdurre un po' di casualità all'interno della tua musica ma, prima di cominciare, dovete conoscere un'importante verità: in Sonic Pi i numeri casuali non sono *veramente casuali*. Che significa? Vediamo insieme.

## Ripetibilità

Una delle funzioni per avere numeri casuali è `rrand` che restituisce un valore compreso tra due numeri, un *minimo* e un *massimo*. (`rrand` è l'abbreviazione di ranged random, ovvero una casualità tra una serie di valori). Proviamo a suonare una nota casuale:

```
play rrand(50, 95)
```

Wow, abbiamo suonato una nota a caso. La nota suonata è `83.7527`. Un numero casuale tra 50 e 95. Ehi, un momento, ho appena predetto il valore esatto della nota casuale che hai suonato anche tu? Sta succedendo qualcosa di strano. Prova a eseguire nuovamente il codice. È uscito ancora `83.7527`? Non può essere casuale!

La risposta è che non è realmente casuale, è pseudo-random. Sonic Pi restituirà sempre dei valori pseudo-casuali in un modo ripetibile. Questo è utile per essere sicuri che la musica create sulla nostra macchina suonerà identica su qualsiasi altro computer, anche utilizzando un po' di casualità nella nostra composizione.

Ovviamente, in un brano musicale, se venisse scelto in modo "casuale" sempre il valore `83.7527` non sarebbe interessante. Comunque non sempre restituirà quel risultato. Prova con il seguente codice:

```
loop do
  play rrand(50, 95)
  sleep 0.5
end 
```

Evviva! Finalmente suona casuale. Se richiamiamo più volte la funzione random, otterremo in cambio valori davvero casuali. Ricorda comunque che al successivo avvio, otterremo la stessa sequenza di numeri casuali per cui suonerà esattamente nello stesso modo. È come se il codice scritto in Sonic Pi andasse indietro nel tempo tornando allo stesso punto di inizio ogni volta che premiamo il pulsante di avvio. È il giorno della marmotta nella sintesi musicale!

## Campane stregate

Un bell'esempio dell'utilizzo di valori casuali è nel seguente codice, chiamato "Campane stregate" che esegue una ripetizione del campione `:perc_bell` con valori casuali di velocità (rate) e pausa (sleep) tra i vari suoni di campana:

```
loop do
  sample :perc_bell, rate: (rrand 0.125, 1.5)
  sleep rrand(0.2, 2)
end
```

## Cutoff (taglio di frequenze) casuale

Un altro esempio sull'utilizzo di valori casuali può essere applicato al taglio di frequenze su un sintetizzatore. Un ottimo synth con cui provarlo è l'emulatore `:tb303`:

```
use_synth :tb303
loop do
  play 50, release: 0.1, cutoff: rrand(60, 120)
  sleep 0.125
end
```

## Semi casuali

E se non ci piacesse la sequenza casuale generata da Sonic Pi? Ovviamente è possibile scegliere un punto di partenza diverso utilizzando `use_random_seed`. Il seme di default è 0; puoi utilizzare semi differenti per ottenere effetti casuali diversi!

Prova con questo esempio:

```
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Ogni volta che avvierai questo codice, sentirai la stessa sequenza di 5 note. Per avere una sequenza differente, proviamo a cambiare il seme:

```
use_random_seed 40
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Questo codice produrrà una sequenza di 5 note diversa dalla precedente. Cambiando il seme e ascoltando il risultato ottenuto possiamo trovare qualcosa che ci piace e quando lo condivideremo con altre persone, loro ascolteranno sempre la stessa identica cosa.

Allora, diamo un'occhiata ad alcune funzioni casuali.


## choose (scegli)

Un'opzione molto utile è quella di scegliere in oggetto a caso da una lista di oggetti già conosciuti. Per esempio, potrei voler suonare una nota dal seguente elenco: 60, 65, 72. Posso ottenere questo risultato con la funzione 'choose' che mi permette di scegliere un oggetto da una lista. Prima di tutto, devo mettere i miei numeri in una lista che può essere creata inserendo i numeri all'interno di parentesi quadre, separati da virgole. Così: '[60,65,72]'. Successivamente, è sufficiente attivare la funzione 'choose':

```
choose([60, 65, 72])
```

Ascoltiamo che cosa ne è venuto fuori:

```
loop do
  play choose([60, 65, 72])
  sleep 1
end
```

## rrand (Funzione random)

Abbiamo già visto la funzione 'rrand', ma proviamo a riavviarla di nuovo. Il risultato è un numero casuale contenuto all'interno di due valori. Questo significa che non otterremo mai il valore esatto dei due numeri estremi ma sempre numeri contenuti in questo intervallo. I numeri saranno sempre dei numeri decimali, ovvero non dei numeri interi ma piuttosto delle frazioni di un numero. Per esempio, proviamo a osservare i numeri che escono da: 'rrand(20,110)':

* 87.5054931640625
* 86.05255126953125
* 61.77825927734375

## rrand_i (numeri interi casuali)

Può capitare di avere bisogno di un numero casuale che sia intero e non decimale. Qui entra in gioco la funzione `rrand_i`. Funziona in modo simile a `rrand` con la differenza che può restituire il valore minimo e massimo come possibili valori (si tratta, infatti, di una funzione inclusiva). Questi sono degli esempi di valori restituiti da `rrand_i(20, 110)`:

* 88
* 86
* 62

## rand

Questa funzione restituirà un numero compreso tra 0 (incluso) e il valore massimo specificato (escluso). Di default restituirà un valore compreso tra 0 e 1. Ad esempio, può essere usata per scegliere un valore casuale per `amp:`:

```
loop do
  play 60, amp: rand
  sleep 0.25
end
```

## rand_i

Dal momento che `rrand_i` e `rrand` si comportano in modo simile, `rand_i` restituirà un numero intero compreso tra 0 e il valore massimo specificato.

## dice (ndT: dado)

A volte vuoi simulare il lancio di un dato. Questo è un caso specifico della funzione `rrand_i` dove il valore più basso è sempre 1. Quando utilizziamo la funzione `dice` dobbiamo specificare il numero di facce del dado. Un dado comune ha 6 facce, quindi `dice(6)` può restituire i valori 1, 2, 3, 4, 5 oppure 6. Ciononostante, come avviene nei giochi di ruolo fantasy, può capitare di usare dadi a 4 facce, a 12, 20 oppure a 120!

## one_in

Infine può capitare di voler simulare l'uscita del punteggio più alto di un dado (6, nel caso di un dado comune). La funzione `one_in` restituisce true con una probabilità di uno sul numero di facce del dado. Di conseguenza `one_in(6)` restituirà true con una probabilità di 1/6 e false negli altri casi. I valori true e false sono molto utili quando si utilizzano i costrutti `if` di cui parleremo più avanti.

È arrivato il momento di sperimentare un po' di casualità nel tuo codice!
