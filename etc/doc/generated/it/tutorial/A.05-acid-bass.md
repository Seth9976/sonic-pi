A.5 Acid Bass

# Acid Bass

È impossibile ripercorrere la storia della musica elettronica senza riscontrare l'impatto del piccolo sintetizzatore Roland TB-303. È l'ingrediente segreto alle spalle del suono acid bass. I riff creati con il TB-303 si sentono dai primi brani della scena House di Chicago fino ad artisti più recenti come Plastikman, Squarepusher e Aphex Twin.

La cosa interessante è che la Roland non aveva creato la TB-303 per la dance music. Originariamente era pensata come strumento di aiuto per i chitarristi: potevano programmare delle linee di basso per improvvisarci sopra. Purtroppo c'erano una serie di problemi: era difficile da programmare, non aveva dei suoni di basso convincenti e costava parecchio. Decisi a limitare le perdite, Roland ha chiuso la produzione a 10.000 unità vendute dopo aver passato molti anni sugli scaffali dei chitarristi. Presto, però, avrebbero avuto vita nuova nei negozi dell'usato. Quelle TB-303 aspettavano solo di essere riscoperte da una nuova generazione di sperimentatori che hanno cominciato a utilizzarle in modi diversi da quelli pensati da Roland per creare nuovi suoni. E così è nata l'Acid House.

Anche se non è così semplice mettere le mani su una TB-303 originale, puoi far si che il tuo Raspberry Pi ne diventi una grazie alla potenza di Sonic Pi. Apri il programma e scrivi questo codice e premi Run:

```
use_synth :tb303
play :e1
```
    
Acid bass istantaneo! Proviamo a smanettare un po'...

## Modifichiamo quel basso

Per prima cosa, costruiamo un arpeggiatore. Nell'ultimo tutorial abbiamo visto come i riff possano essere semplicemente degli anelli di note suonate in sequenza e ripetute. Creiamo un live loop che faccia esattamente questo:

```
use_synth :tb303
live_loop :squelch do
  n = (ring :e1, :e2, :e3).tick
  play n, release: 0.125, cutoff: 100, res: 0.8, wave: 0
  sleep 0.125
end
```

Guarda ogni linea di codice.

1. Nella prima riga, impostiamo il sintetizzatore di default `tb303` con la funzione `use_synth`.
2. Nella seconda riga, creiamo un live loop chiamato `:squelch` che continua a ripetersi.
3. La terza riga è dove creiamo il nostro riff: un anello di note (E dalla prima alla terza ottava) che suoniamo in ordine con `.tick`. Definiamo `n` affinché rappresenti la nota corrente del riff. Il simbolo di uguale significa: assegna il valore a destra al nome sulla sinistra. Questa sarà diversa ogni volta che il loop si ripete. La prima volta `n` sarà `:e1`, la seconda `:e2` seguita da `:e3` e poi dall'inizio.
4. La quarta linea è dove effettivamente facciamo suonare il synth `:tb303`. Passiamo una serie di parametri interessanti: `release:`, `cutoff:`, `res:`, e `wave:` che discuteremo più avanti.
5. La quinta linea è per la pausa `sleep` (stiamo chiedendo al nostro loop di ripetersi ogni `0.125` secondi o 8 volte al secondo a 60BPM).
6. La sesta linea è la fine `end` del live loop. Questo dice a Sonic Pi dove finisce il live loop.

Anche se stai ancora cercando di capire che sta succedendo, scrivi il codice sopra e premi Run. Dovresti sentire il battito del ':tb303' in azione. Quindi, ecco dove sta l'azione: cominciamo a fare live coding.

Anche se il loop è ancora attivo, cambia il valore di 'cutoff:' a '110' e premi di nuovo Run. Dovresti sentire il suono divenare un po' più ruvido e gracchiante. Digita '120' e premi Run e poi '130' e di nuovo Run. Ascolta i cambiamenti nel suono provocati da queste variazioni. Infine, prova a mettere un valore più basso, tipo '80'. Quindi poi ripeti questi cambiamenti con i numeri che vuoi e quante volte vuoi. Non preoccuparti, io ti aspetto qui...

Un'altra caratteristica con cui vale la pena giocare è 'res:'. Questo parametro controlla la risonanza del filtro. Una risonanza alta è caratteristica dei suoni bassi un po' acidi. Adesso abbiamo impostato questo parametro a '0.8'. Prova ad aumentarlo progressivamente a '0.85'. '0.90', '0.95'. Se metti il valore di cutoff a '110' o di più, le variazioni si percepiscono meglio. Infine, prova a fare una pazzia e metti '0.999' come valore di risonanza. A valori così alti di risonanza, puoi sentire il filtro passa-alto, di cui imposti il valore di cutoff, che comincia a risuonare così tanto che si metterà a fare suoni esso stesso, senza che tu intervenga!

Infine, per avere variazioni significative del timbro, prova a cambiare 'wave:', ovvero forma d'onda, mettendo '1'. Questo parametro infatti ti permette di variare l'oscillatore che genera il suono. Quello preimpostato è '0', cioè un generatore di onda a dente di sega. Il numero '1' è un generatore di impulsi e infine '2' è un generatore di onda triangolare.

Quindi, adesso puoi senz'altro provare differenti giri di note cambiando i suoni all'interno dell'anello o anche prendere note da scale o accordi. Divertiti con il tuo primo sintetizzatore di bassi acidi.

## Decostruire il TB-303

Il progetto del TB-303 originale è piuttosto semplice. Compe potrai notare dal diagramma che segue, ci sono soltanto 4 parti principali.

![TB-303 Design](../../../etc/doc/images/tutorial/articles/A.05-acid-bass/tb303-design.png)

Il primo è l'oscillatore d'onda, ovvero l'ingrediente base del suono. In questo caso noi abbiamo un oscillatore di onda quadra. Sussessivamente, abbiamo ampiezza dell'inviluppo dell'oscillatore, che controlla l'ampiezza dell'onda quadra nel tempo. Questa dimensione è accessibile da Sonic Pi mediante i parametri 'attack:', 'decay:', 'sustain:' e 'release:'. Per maggiori informazioni, leggi la sezione 2.4 'Durata degli inviluppi' all'interno della guida. Poi facciamo passare la nostra onda attraverso un filtro passa-basso. Questo taglia le frequenze più acute e dà al suono quella risonanza particolare. E qui comincia il bello. Infatti il valore di taglio (cutoff) del filtro è anche determinato dal suo stesso inviluppo! Questo significa che abbiamo un controllo straodinario sul timbro del suono se giochiamo con entrambi gli inviluppi. Dai un'occhiata sotto:

```
use_synth :tb303
with_fx :reverb, room: 1 do
  live_loop :space_scanner do
    play :e1, cutoff: 100, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
    sleep 8
  end
end
```
    
Per ogni opzione di inviluppo abbiamo un valore di taglio (cutoff) equivalente che possiamo impostare all'interno del synth ':tb303'. Quindi, per cambiare il tempo di attacco del valore di taglio possiamo usare il parametro 'cutoff_attack:'. Copia il codice sopra e incollalo in una finestra vuota. Poi premi Run. Sentirai un suono impazzito che ribolle. Quindi, cominciamo adesso a suonare. Prova a cambiare il valore di 'cutoff_attack:' a '1' e poi a '0.5'. Adesso prova '8'.

Come avrai visto, abbiamo fatto passare tutto il segnale attraverso un effetto di reverbero con ':reverb', ottenendo una bella ambientazione sonora. Provane altri per vedere che succede!

## Mettiamo tutto insieme

Infine, ecco qui un brano che ho composto usando le idee di questa guida. Copialo e incollalo dentro una finestra vuota, prova ad ascoltarlo per un po' e poi comincia a fare i tuoi cambiamenti senza fermare il processo, facendo live coding. Ascolta i magnifici suoni che puoi produrre in questo modo! Ci vediamo la prossima volta...

```
use_synth :tb303
use_debug false
 
with_fx :reverb, room: 0.8 do
  live_loop :space_scanner do
    with_fx :slicer, phase: 0.25, amp: 1.5 do
      co = (line 70, 130, steps: 8).tick
      play :e1, cutoff: co, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
      sleep 8
    end
  end
 
  live_loop :squelch do
    use_random_seed 3000
    16.times do
      n = (ring :e1, :e2, :e3).tick
      play n, release: 0.125, cutoff: rrand(70, 130), res: 0.9, wave: 1, amp: 0.8
      sleep 0.125
    end
  end
end
```
