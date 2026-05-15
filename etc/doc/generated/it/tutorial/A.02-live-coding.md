A.2 Programmare Live

# Programmare dal vivo

I raggi laser attraverso il fumo, il subwoofer che pompa i bassi nella folla. L'atmosfera riempita da sintetizzatori e persone che ballano. Tuttavia c'è qualcosa che non va in questo club. Proiettato sopra alla testa del DJ c'è del testo e non dei normali visual. È la proiezione di Sonic Pi che sta girando su Raspberry Pi. La persona che occupa la cabina del DJ non sta usando dei dischi ma sta scrivendo e modificando del codice. Live. Questo è il Live Coding.

![Sam Aaron Live Coding](../../../etc/doc/images/tutorial/articles/A.02-live-coding/sam-aaron-live-coding.png)

Questa può sembrare una storia di una notte in un club futuristico ma programmare musica è un trend in crescita ed è spesso descritto come Live Coding (http://toplap.org). Uno delle direzioni più recenti che ha preso questo approccio alla musica è l'Algorave (http://algorave.com), eventi dove gli artisti come me programmano musica affinché le persone possano ballare. Non è necessario, però, che tu sia in un nightclub per fare del live coding; con Sonic Pi v.2.6+ puoi farlo ovunque ti porti il tuo Rasberry Pi, un paio di cuffie e delle casse. Quando raggiungerai la fine di questo articoli, saprai programmare e modificare il tuo personalissimo beat. Il punto di arrivo dipenderà solo dalla tua immaginazione.

## Live Loop

La chiave per programmare live con Sonic Pi è capire il funzionamento di `live_loop`. Proviamo a vederne uno:

```
live_loop :beats do
  sample :bd_haus
  sleep 0.5
end
```

Ci sono 4 ingredienti principali nel `live_loop`. Il primo: il suo nome. Il nostro `live_loop` qui sopra si chiama `:beats`. Sei libero di chiamare il tuo `live_loop` come vuoi. Sii creativo. Io spesso utilizzo nomi che comunichino qualcosa riguardo alla musica che stanno producendo alle persone tra il pubblico. Il secondo ingrediente è la parola `do` che indica dove comincia il `live_loop`; il terzo è la parola `end` che indica dove finisce e, infine, c'è il corpo del `live_loop` che descrive quello che verrà ripetuto: tutto ciò che è compreso tra `do` e `end`. In questo caso stiamo suonando in modo ripetitivo un campione di cassa della batteria con mezza misura di pausa. Questo produce un beat regolare. Vai avanti, copia il codice in un buffer vuoto in Sonic Pi e premi run. Boom, boom, boom!.

## Ridefinire on-the-fly

Ok, cosa c'è di così importante in `live_loop`? Fin qui sempre un normale `loop`! Beh, la bellezza del `live_loop` è che puoi modificarli al volo. Questo significa che mentre stanno ancora andando, puoi cambiare il loro comportamento. Questo è il segreto del live coding in Sonic Pi. Proviamo:

```
live_loop :choral_drone do
  sample :ambi_choir, rate: 0.4
  sleep 1
end
```

Ora premi il pulsante Run oppure la scorciatoia `Alt-r`. Ora stai ascoltato il bellissimo suono di un coro. Ora, mentre sta suonando, cambia il valore rate da `0.4` a `0.38`. Premi Run di nuovo. Hai sentito il cambio di nota? Cambialo di nuovo a `0.4` per farlo tornare com'era. Ora, abbassalo a `0.2, poi a `0.19` e poi riportalo a `0.4. Guarda come cambiando un parametro al volo hai il controllo della musica? Ora prova a modificare lo stesso parametro a tuo piacimento. Prova anche con numeri negativi. Divertiti!

## Il tempo di sleep è importante

Una delle lezioni più importanti riguardo a `live_loop` è che devono riposarsi. Guarda questo codice:

```
live_loop :infinite_impossibilities do
  sample :ambi_choir
end
```

Se provi a far funzionare questo codice, Sonic Pi si lamenterà subito che `live_loop` non si ferma. Questo è un sistema di sicurezza! Prenditi un secondo per analizzare cosa questo codice sta chiedendo al computer di fare. Giusto, sta chiedendo di suonare un numero infinito di campioni di coro senza pausa. Senza il sistema di sicurezza il computer proverebbe ad eseguirlo e crasherebbe nel tentativo. Ricorda: i `live_loop` devono contenere `sleep`.


## Combinare suoni

La musica è fatta di cose che accadono nello stesso momento. Batteria, basso, voce, chitarre... tutto in contemporanea. In informatica questo lo chiamiamo concordanza e Sonic Pi lo fa in modo molto semplice. È sufficiente usare più di un `live_loop`!

```
live_loop :beats do
  sample :bd_tek
  with_fx :echo, phase: 0.125, mix: 0.4 do
    sample  :drum_cymbal_soft, sustain: 0, release: 0.1
    sleep 0.5
  end
end
live_loop :bass do
  use_synth :tb303
  synth :tb303, note: :e1, release: 4, cutoff: 120, cutoff_attack: 1
  sleep 4
end
```

Qui abbiamo due `live_loop`, uno che crea beat velocemente e un altro, più lento, che produce strani suoni di basso.

Una delle cose interessanti riguardo i `live_loop` è che possono gestire individualmente il loro tempo. Questo vuol dire che è molto semplice creare strutture poliritmiche interessanti o suonare sfasati nello stile di Steve Reich. Ascolta questo:

```
# Piano Phase di Steve Reich
notes = (ring :E4, :Fs4, :B4, :Cs5, :D5, :Fs4, :E4, :Cs5, :B4, :Fs4, :D5, :Cs5)
live_loop :slow do
  play notes.tick, release: 0.1
  sleep 0.3
end
live_loop :faster do
  play notes.tick, release: 0.1
  sleep 0.295
end
```

## Mettiamo tutto insieme

In ognuno di questi tutorial, finiremo con un esempio di musica che deriva dalle idee che abbiamo introdotto oggi. Leggi il codice e cerca di immaginare il risultato. Poi copialo in un buffer vuoto di Sonic Pi e premi Run e ascolta come suona. Infine, cambia uno dei numeri o utilizza i commenti. Prova a capire se puoi usarli come punti di partenza per una performance e, soprattutto, divertiti! A presto...

```
with_fx :reverb, room: 1 do
  live_loop :time do
    synth :prophet, release: 8, note: :e1, cutoff: 90, amp: 3
    sleep 8
  end
end
live_loop :machine do
  sample :loop_garzul, rate: 0.5, finish: 0.25
  sample :loop_industrial, beat_stretch: 4, amp: 1
  sleep 4
end
live_loop :kik do
  sample :bd_haus, amp: 2
  sleep 0.5
end
with_fx :echo do
  live_loop :vortex do
    # use_random_seed 800
    notes = (scale :e3, :minor_pentatonic, num_octaves: 3)
    16.times do
      play notes.choose, release: 0.1, amp: 1.5
      sleep 0.125
    end
  end
end
```
