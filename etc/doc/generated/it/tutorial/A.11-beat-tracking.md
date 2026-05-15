A.11 Tick Tock

# Tenere traccia del beat

Il mese scorso in questa serie abbiamo parlato in modo approfondito della casualità alla base di Sonic pi. Abbiamo capito come poterla sfruttare in modo deterministico per aggiungere nuovi livelli di controllo del nostro codice. Questo mese continueremo questa analisi approfondendo il sistema di tick di Sonic Pi. Alla fine di questo articolo utilizzerai i tick per i tuoi ritmi e nei tuoi riff per diventare un DJ live coder.

## Contrare i battiti

Quando facciamo musica spesso vogliamo che accada qualcosa in base al beat in cui ci troviamo. Sonic Pi ha un sistema di conteggio chiamato `tick` per avere totale controllo di dove ci troviamo nella battuta e, ovviamente, supporta beat multipli ciascuno col suo tempo.

Diamoci da fare, per avanzare nel beat dobbiamo solo chiamare `tick`. Apriamo un buffer vuoto, scriviamo il seguente codice e premiamo su Run:

```
puts tick #=> 0
```

Ci verrà restituito il beat corrente, quindi `0`. Nota come anche premendo Run più volte il risultato sarà sempre `0`. Questo perché ad ogni avvio, il beat riparte da 0. Tuttavia, mentre Run è ancora attivo, possiamo avanzare nel beat tutte le volte che vogliamo:

```
puts tick #=> 0
puts tick #=> 1
puts tick #=> 2
```

Quando vedi il simbolo `#=>` alla fine di una riga vuol dire che quella linea di codice restituirà del testo nel log nella parte destra. Per esempio `puts foo #=> 0` significa che il codice `puts foo` scriverà `0` nel log.

## Controllare il beat

Abbiamo visto che `tick` fa due cose: incrementa di uno il beat e restituisce il valore corrente. A volte vogliamo solo vedere il valore corrente senza incrementalo. Possiamo farlo usando `look`:

```
puts tick #=> 0
puts tick #=> 1
puts look #=> 1
puts look #=> 1
```

In questo codice usiamo il tick per aumentare la pulsazione due volte e poi usiamo `look`, anch'esso due volte. Nel log vedremo questi valori:  `0`, `1`, `1`, `1`. I primi due tick restituiscono `0` e `1` come ci aspettiamo i due `look` restituiscono l'ultimo valore due volte, ovvero `1`.


## Anelli

Quindi ora possiamo avanzare nel tempo con `tick` e controllare il beat con `look`. Cosa possiamo fare ancora? Dobbiamo avere qualcosa con cui usare tick. Sonic Pi utilizza gli anelli (ring) per rappresentare i riff, melodie e ritmi e il sistema dei tick è stato pensato per lavorare con essi. Di fatto gli anelli hanno la loro versione di `tick` che fa due cose. Innanzitutto funziona come un tick normale e incrementa il beat. In secondo luogo guarda il valore del ring utilizzano il valore del beat come indice. Diamo un'occhiata:

```
puts (ring :a, :b, :c).tick #=> :a
```

`.tick` è una versione speciale di `tick` che ritornerà il primo valore del ring `:a`. Possiamo prendere ciascun valore del ring chiamando `.tick` più volte:

```
puts (ring :a, :b, :c).tick #=> :a
puts (ring :a, :b, :c).tick #=> :b
puts (ring :a, :b, :c).tick #=> :c
puts (ring :a, :b, :c).tick #=> :a
puts look                   #=> 3
```

Prova a guarda il log e vedrai `:a`, `:b`, `:c` e poi di nuovo `:a`. Nota come `look` restituisce `3`. Le chiamate a `.tick` funzionano come normali chiamate di `tick`, incrementano il beat locale.


## Un arpeggiatore con Live Loop

La vera potenza viene fuori quando utilizziamo `tick` con ring e `live_loop`. Quando combiniamo queste cose abbiamo tutti gli strumenti per costruite un semplice arpeggiatore. Abbiamo bisogno di quattro cose:

1. Un anello (ring) che contenga le note che vogliamo ripetere.
2. Un mezzo per incrementare e ottenere il valore del beat.
3. La possibilità di suonare una nota basata sul beat corrente.
4. Una struttura di loop per continuare a ripetere l'arpeggiatore.

Tutti questi concetti possono essere ritrovati in questo codice:

```
notes = (ring 57, 62, 55, 59, 64)
live_loop :arp do
  use_synth :dpulse
  play notes.tick, release: 0.2
  sleep 0.125
end
```

Guardiamo ciascuna linea di codice. Per prima cosa definiamo un ring di note che continuerà a suonare. Dopodiché creiamo un `live_loop` chiamato `:arp` che continuerà a ripetersi. Ogni volta che si ripete il `live_loop`, impostiamo il synth per essere `:dpulse` e poi suonando la nota successiva usando `.tick`. Ricorda che questo incrementerà il nostro contatore di beat e userà l'ultimo valore di beat come indice nel nostro anello. Alla fine, aspettiamo per 1/8 di beat prima di ricominciare da capo.

## Beat multipli simultanei

Una cosa importante da sapere è che i `tick` valgono localmente nel `live_loop`. Questo significa che ciascun `live_loop` ha il suo contatore beat indipendente. Questa cosa è molto più potente che avere un metronomo globale e un beat. Proviamo a vederlo in azione:

```
notes = (ring 57, 62, 55, 59, 64)
with_fx :reverb do
  live_loop :arp do
    use_synth :dpulse
    play notes.tick + 12, release: 0.1
    sleep 0.125
  end
end
live_loop :arp2 do
  use_synth :dsaw
  play notes.tick - 12, release: 0.2
  sleep 0.75
end
```

## Scontri tra beat

Una cosa che causa confusione con il sistema di tick di Sonic Pi è che le persone vogliono utilizzare il tick in più ring nello stesso `live_loop`:

```
use_bpm 300
use_synth :blade
live_loop :foo do
  play (ring :e1, :e2, :e3).tick
  play (scale :e3, :minor_pentatonic).tick
  sleep 1
end
```

Anche se ciascun `live_loop` ha il suo beat counter indipendente, stiamo chiamando `.tick` due volte all'interno dello stesso `live_loop`. Questo significa che il beat verrà incrementato due volte ogni loop. Questo può generare dei poliritmi interessanti ma spesso non è quello che vogliamo. Ci sono due soluzioni per questo problema. Una è chiamare `tick`all'inizio del `live_loop` per poi usare `.look` per controllare il valore; la seconda è dare un nome a ogni chiamata `.tick` come, ad esempio `.tick(:foo)`. Sonic Pi creerà e terrà traccia di un contatore di beat per ciascun nome che utilizzerai. In questo modo puoi lavorare con quanti beat tu voglia! Prova a vedere la sezione 9.4 dei tutorial per maggiori informazioni.

## Mettiamo tutto insieme

Mettiamo insieme tutto quello che abbiamo imparato su `tick`, `ring` e `live_loop` per un ultimo esempio. Come al solito, non trattarlo come un pezzo finito. Prova a cambiare le cose e a giocare con questo codice per vedere cosa riesci a combinare. Alla prossima...

```
use_bpm 240
notes = (scale :e3, :minor_pentatonic).shuffle
live_loop :foo do
  use_synth :blade
  with_fx :reverb, reps: 8, room: 1 do
    tick
    co = (line 70, 130, steps: 32).tick(:cutoff)
    play (octs :e3, 3).look, cutoff: co, amp: 2
    play notes.look, amp: 4
    sleep 1
  end
end
live_loop :bar do
  tick
  sample :bd_ada if (spread 1, 4).look
  use_synth :tb303
  co = (line 70, 130, steps: 16).look
  r = (line 0.1, 0.5, steps: 64).mirror.look
  play notes.look, release: r, cutoff: co
  sleep 0.5
end
```
