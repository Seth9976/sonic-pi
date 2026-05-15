A.10 Controlli

# Controllare il tuo suono

Fino ad ora ci siamo concentrati su come far partire suoni. Abbiamo scoperto che possiamo usare i synth inclusi in Sonic Pi utilizzando `play` o `synth` e come far suonare suoni pre-registrati con `sample`. Abbiamo anche imparato come aggiungere degli effetti a questi suoni come, ad esempio, il riverbero, utilizzando il comando `with_fx`. Combinando tutte queste cose con la gestione del tempo di Sonic Pi, puoi produrre una grande quantità di suoni, beat e riff. Però, una volta lanciata un'opzione di un suono, non c'è modo di controllarla, giusto? Sbagliato! Oggi impareremo qualcosa di molto importante: come controllare i synth mentre suonano.

## Un suono semplice

Creiamo un suono semplice. Apri Sonic Pi e scrivi questo codice:

```
synth :prophet, note: :e1, release: 8, cutoff: 100
```

Ora premi il pulsante Run in alto a sinistra per ascoltare il suono. Ok, premilo un altro paio di volte per sentire come suona. Fatto? Bene, ora cominciamo a controllarlo!

## Nodi dei synth

Una cosa di Sonic Pi che conoscono in pochi è che le funzioni `play`, `synth` e `sample` restituiscono una cosa chiamata `SynthNode` che rappresenta un suono riprodotto. È possibile controllare questi `SynthNode` usando una variabile e poi **controllarla** successivamente. Proviamo a cambiare il valore di `cutoff:` dopo 1 beat:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
control sn, cutoff: 130
```

Guardiamo ogni linea di codice:

Per prima cosa abbiamo suonato il synth `:prophet` utilizzando la normale funzione `synth`. Abbiamo, però, catturato il risultato in una variable chiamata `sn`. Avremmo potuto usare qualsiasi nome come, ad esempio, `synth_note`, o `jane`. Non avrebbe fatto differenza. È importante, però, scegliere nomi che abbiano un senso per la performance e per le persone che leggono il tuo codice. Io ho scelto `sn` perché è diminutivo facile da ricordare per synth node.

Nella seconda linea abbiamo un comando `sleep` normalissimo; non fa nulla di speciale se non dire al computer di aspettare 1 beat prima di andare avanti.

La linea 3 è dove comincia il divertimento. Ora usiamo la funzione `control` per dire al nostro `SynthNode` di cambiare il valore di cutoff a `130`. Se premi **Run** sentirai il `:prophet` suonare come prima ma, dopo 1 beat, il suono diventerà molto più chiaro.

Opzioni modulabili

La maggior parte dei valori dei synth e degli effetti di Sonic Pi possono essere cambiati. Non è così per tutto, però. Per esempio i valori di inviluppo: `attack:`, `decay:`, `sustain:` e `release:` possono essere impostati prima dell'avvio del synth. Capire quali opzioni è possibile cambiare in corso d'opera è semplice, controlla nella documentazione per cercare la frase: "Può cambiare mentre sta suonando" oppure "Non può essere cambiato una volta impostato". Per esempio guardando la documentazione relativa al parametro `attack:` del synth `:beep` dice chiaramente che non può essere modificato:

* Default: 0
* Deve essere più grande di 0
* Non può essere modificato una volta impostato
* Scalato con il valore BPM corrente

## Cambi multipli

Finché un synth sta suonando, non sei limitato al cambiarlo una volta sola; puoi farlo quante volte vuoi. Per esempio possiamo trasformare il nostro `:prophet` in un piccolo arpeggiatore così:

```
notes = (scale :e3, :minor_pentatonic)
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
16.times do
  control sn, note: notes.tick
  sleep 0.125
end
```

In questo codice, abbiamo aggiunto un paio di cose in più. Per prima cosa abbiamo una variabile chiamata `notes` che contiene le note che vogliamo riprodurre (arpeggiatore è il nome carino che viene dato a qualcosa che suona delle note in ciclo). Seconda cosa: rimpiazziamo la nostra chiamata a `control` con un'interazione di 16 volte. In ogni chiamata a `control` usiamo `.tick`per il nostro anello di `notes` che si ripeterà automaticamente. Per un po' di varietà, prova a sostituire `.tick` con `.choose` e vedi se senti la differenza.

Ora che puoi controllare più parametri simultaneamente, prova a cambiare la linea di controllo con questo codice:

```
control sn, note: notes.tick, cutoff: rrand(70, 130)
```

## Sliding

Quando controlliamo un `SynthNode`, risponde esattamente a tempo cambiando immediatamente il valore di un parametro come se avessi premuto un bottone. Questo può dare un suono percussivo, specialmente se il parametro controlla un aspetto del timbro come il `cutoff:`. Tuttavia a volte non vuoi che il cambiamento sia istantaneo; a volte vorresti che fosse graduale, come utilizzando uno slider. Ovviamente Sonic Pi ti permette di farlo usando il parametro `_slide:`.

Ogni parametro che può essere modificato ha un corrispondente con `_slide:` che permette di impostare un tempo. Per esempio `amp:` ha `amp_slide:` e `cutoff:` ha `cutoff_slide:`. Questi parametri funzionano in modo leggermente diverso dagli altri perché dicono al synth come comportarsi **la volta successiva che vengono controllati**. Diamo un'occhiata:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 70, cutoff_slide: 2
sleep 1
control sn, cutoff: 130
```

Nota come questo esempio sia esattamente come lo stesso di prima con la sola aggiunta di `cutoff_slide:`. Questo significa che quando modificheremo il parametro `cutoff:` impiegherà 2 secondi per andare dal valore attuale al successivo. Possiamo usare `control` per sentire lo slide tra 70 e 130. Questo crea una dinamica interessante del suono. Ora prova a cambiare il valore `cutoff_slide:` su un intervallo di tempo più breve come 0.5 oppure più lungo come 4 e vedi come cambia il suono. Ricorda che puoi usare slide per ogni parametro modificabile e ciascuno `_slide:` può avere parametri differenti per cui puoi avere un cutoff lento e un amp veloce e un pan che sta tra i due.

## Mettiamo tutto insieme

Proviamo a guardare a un breve esempio che dimostra la potenza del controllo dei synth. Nota come puoi controllare anche gli effetti cambiando leggermente la sintassi. Rileggiti la sezione 7.2 dei tutorial per maggiori informazioni sugli effetti.

Copia il codice su un buffer vuoto e ascoltalo. Non fermarlo, metti mano al codice. Cambia i tempi di slide, cambia le note, i synth, gli effetti e i tempi di sleep. Vedi se riesci a creare qualcosa di totalmente differente!

```
live_loop :moon_rise do
  with_fx :echo, mix: 0, mix_slide: 8 do |fx|
    control fx, mix: 1
    notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle
    sn = synth :prophet , sustain: 8, note: :e1, cutoff: 70, cutoff_slide: 8
    control sn, cutoff: 130
    sleep 2
    32.times do
      control sn, note: notes.tick, pan: rrand(-1, 1)
      sleep 0.125
    end
  end
end
```
