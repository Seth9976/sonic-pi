A.14 Modulazione d'ampiezza

# Modulazione d'ampiezza

Questo mese scopriremo tutti i segreti di uno degli effetti più potenti e flessibili di Sonic Pi: lo `:slicer`. Alla fine di questo articolo avrete imparato come manipolare il volume generale di alcune parti dei suoni programmati dal vivo. Questo vi permetterà di creare nuove strutture ritmiche e timbriche e di allargare le possibilità sonore.

## Affetta quell'amp

Cosa fa esattamente l'effetto `:slicer`? Possiamo pensarlo come qualcuno che gioca con il controllo del volume della vostra TV o impianto hi-fi. Prima di approfondire il discorso, ascoltate il suono profondo del synth `:prophet` generato con questo codice:

```
synth :prophet, note: :e1, release: 8, cutoff: 70
synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
```

Ora inseriamo l'effetto `:slicer`:

```

with_fx :slicer do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Visto come lo slicer agisce? Sembra che silenzi e poi riattivi l'audio con un ritmo regolare. Sembra anche che ':slicer' abbia effetto sul suono generato dal codice scritto tra i blocchi 'do' e 'end'. Puoi controllare la velocità a cui il suono viene spento e riacceso con lopzione 'phase:' che è l'abbreviazione di durata di fase in inglese (phase duration). Il valore predefinito è '0.25', il che significa che lo status cambia 4 volte al secondo quando i BPM sono a 60. Proviamo a mandarlo più veloce:

```
with_fx :slicer, phase: 0.125 do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Ora, prova a suonare con differenti valori di ':phase'. Prova valori più lunghi o più brevi. Guarda cosa accade quando scegli valori molto brevi. Poi, prova anche a cambiare synth, tipo ':beep' oppure ':dsaw' e anche note differenti. Osserva il diagramma seguente per capire come valori di ':phase' diversi cambino il numero di cambiamenti di ampiezza per ogni battito.

![Phase Durations](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_phase_durations.png)

La durata di una fase corrisponde alla durata del tempo per un ciclo on/off. Pertanto valori più piccoli porteranno ad un passaggio molto più veloce da on a off rispetto a valori più grandi. Dei buoni valori di partenza sono `0.125`, `0.25`, `0.5` e `1`.


## Onde di controllo

Di base, l'effetto ':slicer' usa un'onda quadra per cambiare l'ampiezza d'onda nel tempo. Questo è il motivo per cui sentiamo il suono per un periodo e successivamente questo scompare per poi ricomparire e così via. Però l'effetto ':slicer' può usare 4 diverse forme d'onda per modificare il suono, che sono la dente di sega, la triangolare e la cosinusoidale. Dai un'occhiata al diagramma sotto per capire la loro forma. Possiamo anche ascoltare il loro effetto. Per esempio, se digiti il codice seguente userai l'onda cosinusoidale come onda di controllo. Infatti, in questo caso il suono non si spegne e accende a scatti, ma in maniera progressiva e senza brusche interruzioni:

```
with_fx :slicer, phase: 0.5, wave: 3 do
  synth :dsaw, note: :e3, release: 8, cutoff: 120
  synth :dsaw, note: :e2, release: 8, cutoff: 100
end
```

Suona provando a cambiare le forme d'onda attraverso l'opzione 'wave:': '0' sta per dente di sega, '1' per onda quadra, '2' per triangolare e '3' per sinusoidale. Prova anche a variare la fase attraverso 'phase:' a ogni forma d'onda per sentire come cambia il suono.

Ognuna di queste onde può essere invertita utilizzando l'opzione 'invert_wave:' che la ribalta sull'asse y. Per esempio, durante un ciclo normale l'onda a dente di sega parte dall'alto e si dirige lentamente verso il basso raggiungendo il punto in cui immediatamente ritorna al valore massimo. Invertendo la forma d'onda con 'invert_wave:1', l'onda partirà dal valore minimo ed andrà lentamente verso il valore massimo fino a quando ricadrà improvvisamente al valore minimo. Un'altra opzione è 'phase_offset:' per mezzo del quale possiamo far iniziare l'onda da un punto qualsiasi. Il valore inserito deve essere tra '0' e '1'. Quindi, giocando con i valori di 'phase:', wave:', 'invert_wave:' e 'phase_offset:' puoi cambiare, e di molto, il comportamento dell'ampiezza dell'onda nel tempo.

![Phase Durations](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_control_waves.png)


## Imposta i tuoi livelli

Di base, ':slicer' passa dai valore '1' (volume massimo) e '0' (silenzioso). Questi valori possono essere cambiati per mezzo delle opzione 'amp_min' e 'amp_max'. Puoi cambiare questi valori accoppiandoli con un'onda sinusoidale per creare un effetto tremolo:

```
with_fx :slicer, amp_min: 0.25, amp_max: 0.75, wave: 3, phase: 0.25 do
  synth :saw, release: 8
end
```

Succederebbe praticamente la stessa cosa se prendessi il controllo del volume del tuo impianto stereo e lo muovessi in su e giù facendo ondeggiare il suono.


## Probabilità

Una delle caratteristiche più divertenti di ':slicer' è la possibilità di usare la probabilità per decidere se accendere o spegnere lo slicer. Con questa possibilità, il sistema lancia un dado prima di ogni fase e agisce seguendo il valore che viene fuori. Prova a digitare e eseguire quanto segue:

```
with_fx :slicer, phase: 0.125, probability: 0.6  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

Abbiamo ottenuto un interessante ritmo di impulsi. Prova a cambiare l'opzione 'probability:' (probabilità) con un valore tra '0' e '1'. Più ti avvicini allo '0', più i suoni saranno spaziati tra loro perché la probabilità di accensione dell'effetto risulta più bassa.

Il sistema delle probabilità usato negli effetti è lo stesso utilizzato per la randomizzazione in funzioni come 'rand' e 'shuffle'. Sono entrambi completamente deterministici. Ciò significa che tutte le volte che premi Run, sentirai sempre la stessa sequenza per una data probabilità. Se volessi cambiare la sequenza ottenuta puoi usare l'opzione 'seed:' cambiando il seme di riferimento. Funziona esattamente come 'use_random_seed', soltanto in questo caso si agisce su quel particolare effetto e basta.

Infine, con l'opzione 'prob_pos:' puoi cambiare la posizione di riposo dell'onda di controllo quando il test di probabilità fallisce tra il valore '0' e un altro qualsiasi punto:

```
with_fx :slicer, phase: 0.125, probability: 0.6, prob_pos: 1  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

## Affettare i beat

Una cosa davvero molto divertente è usare ':slicer' per affettare un beat di batteria:

```
with_fx :slicer, phase: 0.125 do
  sample :loop_mika
end
```

Questo ci permette di creare nuove possibilità ritmiche utilizzando un qualsiasi campione, il che è piuttosto divertente. Tuttavia, bisogna star attenti a che la durata dei campioni sia coerente con i BPM impostati in Sonic Pi, altrimenti l'azione di affettamento non funzionerà. Per esempio, prova a cambiare da ':loop_mika' a ':loop_amen' e ascolta come suona male quando la durata non coincide.

## Cambiamenti di tempo

Come abbiamo già visto, cambiare i BPM preimpostati con 'use_bpm' farà anche cambiare i tempi di 'sleep' e gli le durate degli inviluppi coerentemente al nuovo valore. L'effetto ':slicer' si comporta nello stesso modo e l'opzione 'phase:' è in realtà misurata in battiti e non in secondi. Quindi possiamo risolvere il problema di 'loop_amen' appena visto cambiando i BPM e farli coincidere con quelli del campione:

```
use_sample_bpm :loop_amen
with_fx :slicer, phase: 0.125 do
  sample :loop_amen
end
```

## Mettiamo tutto insieme

Quindi, applichiamo le idee esposte sopra nell'esempio finale che utilizza soltanto l'effetto ':slicer' per creare una combinazione interessante. Vai avanti, cambia quanto scritto e componi il tuo brano personale!

```
live_loop :dark_mist do
  co = (line 70, 130, steps: 8).tick
  with_fx :slicer, probability: 0.7, prob_pos: 1 do
    synth :prophet, note: :e1, release: 8, cutoff: co
  end
  
  with_fx :slicer, phase: [0.125, 0.25].choose do
    sample :guit_em9, rate: 0.5
  end
  sleep 8
end
live_loop :crashing_waves do
  with_fx :slicer, wave: 0, phase: 0.25 do
    sample :loop_mika, rate: 0.5
  end
  sleep 16
end
```




