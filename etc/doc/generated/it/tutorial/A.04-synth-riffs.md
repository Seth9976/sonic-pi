A.4 Riff di synth

# Riff di synth

Sia che siano i minacciosi scivolamenti i oscillatori rumorosi, sia che siano i colpi scordati di onde a dente di sega che trapuntano il suono, il synth principale gioca un ruolo essenziale in ogni brano elettronico. Nell'edizione del mese scorso, in questa guida abbiamo trattato le modalità di programmazione dei ritmi. In questa edizione tratteremo invece di come programmare tre componenti centrali di un riff fatto con il synth: il timbro, la melodia e il ritmo.

Bene, allora accendiamo il Raspberry Pi, apriamo Sonic Pi in versione 2.6 o più recente, e mettiamoci a fare un po' di rumore!


## Possibilità trimbriche

Una parte essenziale di ogni riff fatto con il synth è di cambiare il timbro dei suoni e giocarci. In Sonic Pi, il timbro può essere controllato in due modi: cambiando totalmente synth per avere delle modifiche più radicali, cambiando le varie caratteristiche del synth che siamo utilizzando per avere delle modifiche più raffinate e precise. Possiamo utilizzare anche FX, ma questo è spiegato in un'altra guida...

Creiamo un semplice live loop dove continuiamo a cambiare il sintetizzatore usato:

```
live_loop :timbre do
  use_synth (ring :tb303, :blade, :prophet, :saw, :beep, :tri).tick
  play :e2, attack: 0, release: 0.5, cutoff: 100
  sleep 0.5
end
```

Dai uno sguardo al codice. Siamo semplicemente scorrendo attraverso una serie di nomi di synth (quando arriverai in fondo, la lista riparte da capo). Ogni nome viene selezionato attraverso la funzione 'use_synth', che cambierà anche il synth utilizzato all'interno del 'live_loop' corrente. Stiamo suonando anche la nota ':e2' (E, o 'Mi' nel sistema italiano, alla seconda ottava), con un rilascio della nota di 0,5 battiti (mezzo secondo con i BPM preimpostati a 60) e il livello di 'cutoff:' impostato a 100.

Ascolta adesso come i diversi synth hanno un suono radicalmente diverso, anche se tutti suonano la stessa nota. Prova a sperimentare, cambiando il tempo di rilascio della nota ('release:') o cambiando anche il tempo di attacco ('attack:') così da capire come questi parametri hanno un forte impatto sul suono. Infine, prova a cambiare il 'cutoff:' per vedere come diversi valori cambiano il timbro (i valori tra 60 e 130 sono i migliori). Fai caso a quanti differenti suoni puoi creare semplicemente modificando alcuni valori. Quando avrai imparato a gestirli, vai alla scheda Synth dentro il sistema Help e qui troverai tutti i synth a disposizione e tutti i parametri che ogni synth supporta. Così ti renderai conto di quanto potenti possono essere le tue dita programmatrici.

## Timbro

Timbro è solo un bella parola per descrivere il suono di un suono. Se, infatti, provi a suonare la stessa nota con strumenti differenti come, ad esempio, il violino, la chitarra, il pianoforte, il pitch (l'altezza della nota) sarà lo stesso ma la qualità sonora sarà diversa. Questa qualità sonora, quella che ti permette di distinguere un pianoforte da una chitarra è il timbro.


## Composizioni melodiche

Un'altro aspetto importante per il nostro synth lead è la scelta delle note che vogliamo suonare. Se hai già un'idea a riguardo, puoi creare semplicemente un ring con le note e utilizzare il tick per scorrere da una all'altra:

```
live_loop :riff do
  use_synth :prophet
  riff = (ring :e3, :e3, :r, :g3, :r, :r, :r, :a3)
  play riff.tick, release: 0.5, cutoff: 80
  sleep 0.25
end
```
    
In questo esempio abbiamo definito la nostra melodia con un anello che include sia note come, ad esempio `:e3` che pause rappresentate da `:r`. Usiamo `.tick` per scorrere tra le note e avere un riff che si ripete.

## Melodia automatica

Spesso non è semplice trovare un buon riff partendo da zero. Spesso è più semplice chiedere a Sonic Pi una selezione di riff casuale per scegliere il migliore. Per fare una cosa del genere, abbiamo bisogno di tre cose: anelli, casualità e seme di casualità. Proviamo a vedere un esempio:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 3
  notes = (scale :e3, :minor_pentatonic).shuffle
  play notes.tick, release: 0.25, cutoff: 80
  sleep 0.25
end
```

Qui succedono un po' di cose... cerchiamo di andare con ordine. Per prima cosa specifichiamo che stiamo usando 3 come seme di casualità. Cosa significa? La cosa importante è sapere che quando utilizziamo un seme, possiamo fare una predizione su quello che sarà il prossimo numero casuale. Un'altro punto importante è che mescolare un anello di note funziona nello stesso modo. Nell'esempio qui sopra stiamo semplicemente chiedendo la il terzo mescolamento nella lista standard dei mescolamenti (che sarà sempre la stessa quando impostiamo lo stesso seme prima di mescolare). Infine utilizziamo il tick per scorrere tra le note per suonare il riff.

Qui comincia il bello. Se cambiamo il seme a, per esempio, 3000, otterremo un mescolamento completamente differente. Così diventa semplice esplorare nuove melodie:è sufficiente scegliere una lista di note che vogliamo mescolare (le scale sono un ottimo punto di partenza) e poi scegliere il seme con cui le vogliamo mescolare. Se non ci piace la melodia, basta cambiare una delle due cose e riprovare finché non sentiamo qualcosa che ci piace.


## Pseudo casualità

La casualità in Sonic Pi non è realmente casuale ma è quello che si chiama pseudo-random. Immagina di lanciare un dado 100 volte e scrivere il risultato di ogni lancio su un foglio di carta. Sonic Pi ha lo stesso numero di possibili risultati quando gli chiediamo un numero casuale ma, invece che lanciare un dado, prendere il valore successivo in lista. Impostando un seme stabiliamo un punto specifico di inizio da quella lista.
 
## Trovando il tuo ritmo

Un altro aspetto importante del riff è il ritmo ovvero quando suonare una nota e quando no. Come abbiamo visto in precedenza, possiamo usare `:r` nel nostro anello per aggiungere le pause. Un'altra possibilità è l'utilizzo degli spread che vedremo in futuro. Oggi continueremo a usare la casualità per trovare il nostro ritmo. Invece che suonare ogni nota, possiamo usare una condizione per far sì che suoni con una certa probabilità. Diamo un'occhiata:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 30
  notes = (scale :e3, :minor_pentatonic).shuffle
  16.times do
    play notes.tick, release: 0.2, cutoff: 90 if one_in(2)
    sleep 0.125
  end
end
```

Una funzione molto utile da conoscere è `one_in` che restituisce il valore `true` o `false` con la probabilità specificata. Qui stiamo usando un valore 2 quindi `one_in` restituirà `true` una volta su due. In altre parole, il 50% delle volte avremo `true`. Utilizzando valori più alti, alzeremo le probabilità che ritorni `false` aumentando gli spazi nel riff.

Da notare che abbiamo usato l'iterazione `16.times`. Questo perché vogliamo resettare il nostro valore di seme ogni 16 così per cui il nostro ritmo si ripeterà per 16 volte. Questo non ha effetti sul mescolamento perché viene eseguito immediatamente appena è impostato il seme. Possiamo modificare le quantità di ripetizioni nell'iterazione per allungare il riff. Prova a cambiare il valore da 16 a 8 oppure 4 o 3 per vedere come cambia il ritmo del riff.

## Mettiamo tutto insieme

Ok, mettiamo insieme tutto quello che abbiamo imparato oggi in un ultimo esempio. Alla prossima!

```
live_loop :random_riff do
  #  togli il cancelletto che serve per i commenti per attivare la funzione:
  #  synth :blade, note: :e4, release: 4, cutoff: 100, amp: 1.5
  use_synth :dsaw
  use_random_seed 43
  notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle.take(8)
  8.times do
    play notes.tick, release: rand(0.5), cutoff: rrand(60, 130) if one_in(2)
    sleep 0.125
  end
end
 
live_loop :drums do
  use_random_seed 500
  16.times do
    sample :bd_haus, rate: 2, cutoff: 110 if rand < 0.35
    sleep 0.125
  end
end
 
live_loop :bd do
  sample :bd_haus, cutoff: 100, amp: 3
  sleep 0.5
end
```
