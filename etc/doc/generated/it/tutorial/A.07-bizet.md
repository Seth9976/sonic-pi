A.7 Battiti di Bizet

# Battiti di Bizet

Dopo la nostra breve trattazione del fantastico mondo del controllo di Minecraft attraverso Sonic Pi dello scorso mese, ricominciamo a fare musica. Oggi, ci occuperemo di creare un fantastico pezzo dance del 21° secolo usando le meravigliose proprietà del codice.

## Oltraggioso e distruttivo

Torniamo indetro al 1875. Un compositore chiamato Bizet aveva appena terminato la sua ultima opera, Carmen. Sfortunatamente, come molti pezzi che rompevano gli schemi preesistenti, le persone non apprezzarono questa opera perché fu considerata oltraggiosa e troppo fuori dai canoni. Purtroppo, Bizet morì dieci anni prima di vedere il suo lavoro salire al successo internazionale, diventando una delle opere più famose ed eseguite della storia della musica. Per onorare questa tragedia, prendiamo una delle melodie più famose della Carmen e rivisitiamola in chiave moderna rendendola anche noi oltraggiosa e differente dai canoni prestabiliti a noi contemporanei. La suoneremo attraverso il live coding!

## Decodificare la Habanera

Suonare con il live coding l'intera opera sarebbe una cosa un po' difficile da far entrare in questa guida, quindi focalizziamoci soltanto sulle parti più famose. In questo caso, prendiamo la linea di basso dell'Habanera:

![Habanera Riff](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera.png)

Questa cosa potrebbe sembrare particolarmente difficile da leggere se non hai studiato la notazione musicale. Tuttavia, noi programmatori consideriamo la notazione musicale come una forma diversa di codice. Infatti essa rappresenta delle istruzioni destinate a un musicista invece che a un computer. Quindi, dobbiamo solo capire come riuscire a decodificarla.

## Note

Le note sono organizzate da destra a sinistra, in modo simile alle parole di questa rivista. Però, esse hanno anche differenti altezze. *La posizione verticale della nota rappresenta anche quanto essa è acuta o grave.* Più è posta in alto nel pentagramma, più la nota sarà acuta.

Abbiamo già visto come cambiare l'altezza di una nota in Sonic Pi. Possiamo usare sia numeri più o meno alti come 'play 75' o 'play 80', sia i nomi delle note in notazione alfabetica come 'play :E' e 'play :F' (ovvero suona Mi e suona Fa in notazione italiana). Fortunatamente, ogni posizione della nota nella partitura rappresenta il nome di una specifica nota. Dai un'occhiata a questa tabella:

![Notes](../../../etc/doc/images/tutorial/articles/A.07-bizet/notes.png)

## Pause

La partitura musicale è un tipo di codice molto ricco ed espressivo, capace di comunicare molte cose. Non dovrebbe essere troppo soprendente quindi scoprire che essa non solo ti dice quale nota suonare ma anche quando non devi suonarne. Nei linguaggi di programmazione, questo aspetto somiglia all'idea di 'nil' o 'null', ovvero l'assenza di un valore. In altre parole, non suonare una nota significa l'assenza di una nota.

Se guardi attentamente la partitura, ti accorgerai che è formata da una combinazione di punti neri dotati di linee che rappresentano la nota da suonare e delle cose strambe che rappresentano le pause. Fortunatamente, Sonic Pi ha una rappresentazione molto pratica delle pause: ':r'. Quindi se eseguiamo l'istruzione 'plat :r', Sonic Pi suonerà il silenzio! Possiamo anche scrivere 'play :rest', 'play nil' o 'play false', che sono tutti equivalenti alla prima.

## Ritmo

Infine c'è un'ultima cosa da imparare: come decodificare la notazione ovvero il timing delle note. Nella notazione originale, puoi vedere che le note sono connesse da linee spesse chiamate codette (o stanghette). La seconda nota ha due stanghette quindi dura 1/16. Le altre note hanno una singola stanghetta che indica una durata di 1/8. La pausa ha anch'essa due stanghette infatti ha una durata di 1/16.

Quando proviamo a decodificare o esplorare nuove cose, un trucco è rendere tutto il più simile possibile per vedere eventuali relazioni o pattern. Per esempio, quando riscriviamo tutto con note da 1/16 puoi notare come la notazione diventi una bella sequenza di note e pause.

![Habanera Riff 2](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera2.png)

## Programmare l'Habanera

Stiamo per tradurre questa linea di basso in Sonic Pi. Codifichiamo queste note e pause in un anello:

```
(ring :d, :r, :r, :a, :f5, :r, :a, :r)
```
    
Proviamo a sentire come suona. Inseriamo tutto in un live loop con un tick per scorrere:

```
live_loop :habanera do
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
```
    
Fantastico, il riff che suona dalle casse è istantaneamente riconoscibile. È stato molto faticoso arrivare fino a qui ma ne valeva la pena. Batti un cinque!
    
## Synth malinconico

Ora che abbiamo la linea di basso, ricreiamo l'ambientazione della scene lirica. Un synth da provare è `:blade` che è un sintetizzatore malinconico degli anni '80. Proviamolo facendo passare la nota iniziale `:d` attraverso uno slicer e un riverbero:

```
live_loop :habanera do
  use_synth :fm
  use_transpose -12
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
with_fx :reverb do
  live_loop :space_light do
    with_fx :slicer, phase: 0.25 do
      synth :blade, note: :d, release: 8, cutoff: 100, amp: 2
    end
    sleep 8
  end
end
```

Ora, proviamo con le altre note nella linea di basso: `:a` e `:f5`. Ricorda: non devi premere stop, puoi modificare il codice mentre la musica sta suonando e premere Run di nuovo. Prova anche a cambiare i valori (ad esempio `0.5`, `0.75` e `1`) del parametro `phase:` dello slicer.

## Mettiamo tutto insieme

Infine, combiniamo tutte le idee che abbiamo avuto fino ad ora per un remix dell'Habanera. Come puoi notare, ho aggiunto un'altra linea di basso come un commento. Una volta scritto tutto in un buffer vuoto, premi Run e senti come suona la composizione. Ora, senza premere stop *togli il commento* dalla seconda linea rimuovendo il `#` e poi premi di nuovo Run. Quando è figo? Ora prova a fare un po' di cambiamenti e divertiti.

```
use_debug false
bizet_bass = (ring :d, :r, :r, :a, :f5, :r, :a, :r)
#bizet_bass = (ring :d, :r, :r, :Bb, :g5, :r, :Bb, :r)
 
with_fx :reverb, room: 1, mix: 0.3 do
  live_loop :bizet do
    with_fx :slicer, phase: 0.125 do
      synth :blade, note: :d4, release: 8,
        cutoff: 100, amp: 1.5
    end
    16.times do
      tick
      play bizet_bass.look, release: 0.1
      play bizet_bass.look - 12, release: 0.3
      sleep 0.125
    end
  end
end
 
live_loop :ind do
  sample :loop_industrial, beat_stretch: 1,
    cutoff: 100, rate: 1
  sleep 1
end
 
live_loop :drums do
  sample :bd_haus, cutoff: 110
  synth :beep, note: 49, attack: 0,
    release: 0.1
  sleep 0.5
end
```
