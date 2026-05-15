A.6 Minecraft Musicale

# Minecraft Musicale


Minecraft Pi


Ciao, bentornato/a! Nella guida precedente ci siamo concentrati maggiormente sulle possibilità musicali di Sonic Pi, ovvero far diventare il tuo Raspberry Pi uno strumento musicale pronto all'uso. Fino ad ora, abbiamo imparato a:

* Fare live coding: cambiare i suoni in tempo reale,
* Scrivere codice per produrre dei bei bassi che pompano,
* Generare delle potenti linee melodiche con i synth,
* Ricreare il suono acido e basso del TB-303.

C'è anche molto altro che vorrei mostrarti e che tratteremo in edizioni future di questa guida. Comunque, questo mese, diamo un'occhiata a qualcosa che Sonic Pi può fare e della quale probabilmente non sei a conoscenza: controllare Minecraft.

## Ciao mondo di Minecraft

Bene, cominciamo allora. Accendi il tuo Raspberry Pi e avvia il tuo Minecraft Pi creando un nuovo mondo. Quando hai fatto, avvia Sonic Pi, ridimensiona e sposta la finestra in modo tale da riuscire a vedere le due finestre contemporaneamente.

In una finestra vuota di Sonic Pi inserisci il codice seguente:

```
mc_message "Hello Minecraft from Sonic Pi!"
```
    
Adesso premi Run. Booom! Il tuo messaggio è apparso dentro Minecraft! E' stato facile, vero? Adesso, smetti per un momento di leggere e mettiti a giocare scrivendo alcuni messaggi a tuo piacimento. Goditela!

![Screen 0](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-0-small.png)

## Teletrasporto sonico

Adesso, mettiamoci un attimo a esplorare. Solitamente utilizziamo il mouse e la tastiera per camminare in giro. Funziona, è vero, ma è piuttosto lento e noioso. Sarebbe molto meglio avere una sorta di macchina del teletrasporto. Beh, grazie a Sonic Pi, ce l'abbiamo. Prova il codice seguente:

```
mc_teleport 80, 40, 100
```
    
Perbacco! Era una strada lunga. Se non fossi stato in modalità volante saresti ricaduto giù a terra. Se tocchi due volte lo spazio per entrare in modalità volo e teletrasportarti di nuovo, rimarrai a fluttuare nel posto che hai toccato.

Allora, cosa significano quei numeri? Abbiamo tre numeri che descrivono le coordinate del posto nel mondo in cui vogliamo andare. Diamo a ogni numero un nome: x, y e z:

* x indica quanto ci spostiamo a destra o sinistra (80 nel nostro esempio)
* y indica quando in alto vogliamo rimanere (40 nel nostro esempio)
* z indica quanto vogliamo andare avanti o indietro (100 nel nostro esempio)

Scegliendo differenti valori per x, y e z possiamo teletrasportarci ovunque nel nostro mondo. Prova! Scegli dei numeri diversi e guarda dove vai a finire. Se lo schermo diventa nero è perché ti sei teletrasportato/a sotto terra o in una montagna. Scegli un numero di y più alto e vedrai che tornerai in superficie. Continua a esplorare finché non trovi un posto che ti piace...

Usa le idee spiegate fino a ora e costruisci un Teletrasporto Sonico che rende divertente spostare dei suoni mentre ci spara in giro nel mondo di Minecraft:

```
mc_message "Preparing to teleport...."
sample :ambi_lunar_land, rate: -1
sleep 1
mc_message "3"
sleep 1
mc_message "2"
sleep 1
mc_message "1"
sleep 1
mc_teleport 90, 20, 10
mc_message "Whoooosh!"
```
    
![Screen 1](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-1-small.png)

## Blocchi magici

Adesso che hai finalmente trovato un posto bello, comincia a costruire. Puoi fare quello che fai di solito e cominciare a cliccare il mouse furiosamente per mettere i blocchi sotto il cursore. Oppure, puoi utilizzare la magia di Sonic Pi. Prova questo codice:

```
x, y, z = mc_location
mc_set_block :melon, x, y + 5, z
```

Guarda in alto! C'è un melone nel cielo! Guarda un attimo il codice. Che cosa abbiamo fatto? Nella prima riga abbiamo preso la posizione attuale di Steve e codificata come variable di x, y e z. Queste lettere corrispondono alle coordinate descritte in precedenza. Abbiamo usato queste coordianate nella funzione 'mc_set_block' che pone il blocco scelto nel luogo indicato dalle coordinate. Per porre qualcosa nel cielo, dobbiamo soltanto incrementare il valore di y. E' per questo che abbiamo aggiunto 5. Proviamo a fare una lunga striscia di meloni:

```
live_loop :melon_trail do
  x, y, z = mc_location
  mc_set_block :melon, x, y-1, z
  sleep 0.125
end
```

Quindi, torniamo a Minecraft, assicurati di essere in modalità volo (tocca due volte spazio se non lo sei) e vola intorno al mondo. Guarda in giù e vedrai una bellissima scia di meloni. Guarda che belle figure intrecciate che puoi fare nel cielo.

## Fare live coding con Minecraft

Coloro che hanno seguito questa guida durante gli ultimi mesi avranno probabilmente la mente già aperta. La scia di meloni è molto bella, ma la parte più interessante dell'esempio precedente è il poter fare un 'live_loop' con Minecraft! Per chi non lo sapesse, 'live_loop' è un'abilità magica di Sonic Pi che nessun altro linguaggio di programmazione ha. Ti permette di far girare numerosi loop contemporaneamente e di cambiarli mentre sono in esecuzione. Sono incredibilmente potenti e fonte di estremo divertimento. Io uso i 'live_loop' per suonare musica nei locali con Sonic Pi; i DJ usano i dischi e io uso i 'live_loop' :-). Comunque, oggi andremo a programmare dal vivo sia la musica sia Minecraft.

Iniziamo. Esegui il codice sopra e inizia nuovamente a costruire la tua scia di meloni. Adesso, senza fermare l'esecuzione, cambia ':melon' con ':brick' (mattone) e premi Run. Bravo! Adesso stai facendo una scia di mattoni. Semplice no?! Vuoi aggiungere anche un po' di musica? Facile, prova questo:

```
live_loop :bass_trail do
  tick
  x, y, z = mc_location
  b = (ring :melon, :brick, :glass).look
  mc_set_block b, x, y -1, z
  note = (ring :e1, :e2, :e3).look
  use_synth :tb303
  play note, release: 0.1, cutoff: 70
  sleep 0.125
end
```
    
Ora, mentre la musica sta suonando, tu comincia a cambiare il codice. Cambia i tipi di blocco: prova ':water', ':grass' o la tua categoria di blocchi preferita. Cambia anche il valore 'cutoff:' da '70' a '80' e successivamente anche a '100'. Non è divertente?

## Mettiamo tutto insieme

![Screen 2](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-2-small.png)

Combiniamo tutto quello che abbiamo visto finora aggiungendo un tocco di ulteriore magia. Associamo la nostra abilità di teletrasportarci, il posizionamento dei blocchi e la musica per fare una sorta di video musicale di Minecraft. Non ti preoccupare se non riesci a capire tutto. Digita soltanto il testo e divertiti cambiando alcuni valori mentre il codice è in esecuzione. Divertiti e ci vediamo la prossima volta...
    
```
live_loop :note_blocks do
  mc_message "This is Sonic Minecraft"
  with_fx :reverb do
    with_fx :echo, phase: 0.125, reps: 32 do
      tick
      x = (range 30, 90, step: 0.1).look
      y = 20
      z = -10
      mc_teleport x, y, z
      ns = (scale :e3, :minor_pentatonic)
      n = ns.shuffle.choose
      bs = (knit :glass, 3, :sand, 1)
      b = bs.look
      synth :beep, note: n, release: 0.1
      mc_set_block b, x+20, n-60+y, z+10
      mc_set_block b, x+20, n-60+y, z-10
      sleep 0.25
    end
  end
end
live_loop :beats do
  sample :bd_haus, cutoff: 100
  sleep 0.5
end
```
