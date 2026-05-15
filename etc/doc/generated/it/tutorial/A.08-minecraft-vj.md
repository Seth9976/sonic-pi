A.8 Diventa un VJ di Minecraft

# Diventa un DJ di Minecraft

![Screen 0](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-0-small.png)


Minecraft Pi


Tutti abbiamo giocato a Minecraft. Probabilmente avrai costruito strutture grandiose, disegnato trappole e creato linee di carrelli elaborate attraverso gli switch. Ma quanti di voi si sono esibiti con Minecraft? Scommetto che non sapevate di poter usare Minecraft per creare visual incredibili come un VJ professionista.

Se l'unico modo di modificare Minecraft fosse il mouse, faresti molta fatica a cambiare le cose velocemente. Per fortuna, il tuo Raspberry Pi ha una versione di Minecraft che può essere controllare con il codice. E c'è anche un'app chiamata Sonic Pi che rende programmare Minecraft facile e divertente.

Nell'articolo di oggi, ti mostrerò qualche trucchetto che abbiamo usato per esibirci in night club e locali in giro per il mondo.

Cominciamo...

## Cominciamo

Partiamo da un semplice esercizio di riscaldamento per ricordarci le basi. Per prima cosa, apri sul tuo Raspberry Pi sia Minecraft che Sonic Pi. In Minecraft crea un nuovo mondo e in Sonic Pi scegli un buffer vuoto e incolla questo codice:

```
mc_message "Let's get started..."
```
    
Premi Run e vedrai un messaggio nella finestra di Minecraft. Ok, siamo pronti per partire e divertici...

## Tempesta di sabbia

Quando usiamo Minecraft per creare visual, dobbiamo pensare a qualcosa che sia interessante da vedere e semplice da generare. Un semplice trucco è creare una tempesta di sabbia facendo cadere blocchi di sabbia dal cielo. Per farlo, abbiamo bisogno di semplici funzioni:

* `sleep` - per inserire una pausa tra le azioni
* `mc_location` - per trovare la nostra posizione attuale
* `mc_set_block`- per piazzare blocchi di sabbia in una posizione specifica
* `rrand` - per generare valori casuali all'interno di un range
* `live_loop` - per avere una continua pioggia di sabbia

Se non sei a conoscenza di qualcuna di queste funzioni come, ad esempio, `rrand`, scrivi la parola nel buffer, fai click su di essa e poi premi `Control-i` per aprire la documentazione relativa. In alternativa puoi accedere alla tab *lang* nell'aiuto e cercare direttamente la funzione.

Proviamo con un po' di pioggia prima di fare una vera tempesta. Otteniamo la posizione correte per creare qualche blocco di sabbia nel cielo:

```
x, y, z = mc_location
mc_set_block :sand, x, y + 20, z + 5
sleep 2
mc_set_block :sand, x, y + 20, z + 6
sleep 2
mc_set_block :sand, x, y + 20, z + 7
sleep 2
mc_set_block :sand, x, y + 20, z + 8
```
    
Quando premi Run prova a guardarti intorno perché i blocchi potrebbero scendere dietro di te in base a dove stai guardando. Non ti preoccupare, se te li sei persi, premi Run di nuovo... basta essere sicuri di guardare nella giusta direzione!

Analizziamo velocemente cosa sta succedendo. Nella prima linea abbiamo preso la posizione di Steve come coordinate per la funzione `mc_location` e le abbiamo inserite nelle variabili `x`, `y`, e `z`. Poi, nella riga successiva, abbiamo usato `mc_set_block` per piazzare un po' di sabbia nelle stesse coordinate con una leggera modifica: abbiamo scelto la stessa x mentre y e z con valori di 20 blocchi maggiori per far si che i blocchi di sabbia cadessero in linea lontano da Steve.

Perché non provi a modificare un po' questo codice? Aggiungi un po' di righe, cambia i tempi di pausa, prova a modificare `:sand` con `:gravel` e scegli coordinate different. Sperimenta e divertiti!

## Liberiamo il Live Loop

Ok, è tempo di fare un vera e propria tempesta utilizzando la potenza di `live_loop`, una delle funzioni magiche di Sonic Pi che ci permette di esplorare tutta la potenza del live coding: modificare il codice mentre sta funzionando!

```
live_loop :sand_storm do
  x, y, z = mc_location
  xd = rrand(-10, 10)
  zd = rrand(-10, 10)
  co = rrand(70, 130)
  synth :cnoise, attack: 0, release: 0.125, cutoff: co
  mc_set_block :sand, x + xd, y+20, z+zd
  sleep 0.125
end
```
    
Che divertimento! Stiamo ripetendo tutto abbastanza velocemente (8 volte al secondo) e ad ogni loop cerchiamo la posizione di Steve ma stavolta generiamo 3 valori casuali:

* 'xd' - quando x è tra -10 e 10
* 'zd' - quando anche z è tra -10 e 10
* 'co' - è un valore di taglio di un filtro passabasso e può stare tra 70 e 130

Quindi usiamo quei valori casuali nelle funzioni 'synth' e 'mc_set_block' per creare della sabbia che cade in luoghi casuali intorno a Steve insieme a un suono percussivo di pioggia prodotto dal synth ':cnoise'.

Per quelli e quelle che ancora non conoscono i live loop, posso dirvi che è proprio qui che ci si comincia a divertire con Sonic Pi. Mentre il codice è in esecuzione e la sabbia sta cadendo, prova a cambiare uno dei valori, magari il tempo di sleep a '0.25' oppure la tipologia di blocco da ':sand' a ':gravel'. Premi di nuovo Run. Hey, facile! Le cose sono cambiate senza arrestare l'esecuzione. Questo è il tuo primo passo nella carriera di VJ! Continua a fare pratica e a cambiare le cose. Quanto puoi cambiare le immagini senza arrestare l'esecuzione del codice?

## Combinazioni di blocchi epiche

![Screen 1](../../../etc/doc/images/tutorial/articles/A.08-minecraft-vj/minecraft-vj-1-small.png)

Infine, un altro modo molto divertente di generare delle immagini interessanti è di creare enormi muri con diverse combinazioni di blocchi sopra cui volare e avvicinarsi. Per creare questo effetto, dobbiamo smettere di posizionare i blocchi casualmente, mettendoli invece in maniera ordinata. Possiamo farlo intrecciando due gruppi di iterazioni diverse (premi il tasto Help e vai alla sezione 5.2, "Iterazione e loop" per maggiori informazioni sull'iterazione). Il valore '|xd|' posizionato dopo il 'do' significa che 'xd' sarà il valore stabilito per ogni iterazione. Quindi, la prima volta sarà 0, poi 1, 2 e così via. Intrecciando due iterazioni tra loro in questo modo possiamo generare tutte le coordinate di un quadrato. Così, poi possiamo scegliere casualmente i tipi di blocco per creare degli effetti interessanti:

```
x, y, z = mc_location
bs = (ring :gold, :diamond, :glass)
10.times do |xd|
  10.times do |yd|
    mc_set_block bs.choose, x + xd, y + yd, z
  end
end
```

Abbastanza chiaro. Mentre ci divertiamo qui, proviamo a cambiare 'bs.choose' con 'bs.tick' per spostarci da uno schema casuale a uno più regolare. Prova a cambiare anche il tipo di blocco e i più coraggiosi possono anche provare a includere tutto in un 'live_loop' così che la struttura continuerà a cambiare automaticamente.

Quindi, per un gran finale da VJ, cambia i due '10.times' con '100.times' e premi Run. Booom! Un muro gigantesco di blocchi posti casualmente! Immaginati quando tempo potresti impiegare a costruire una cosa del genere usando solo il mouse! Premi due volte lo spazio per entrare in modalità volante e comincia a sorvolare tutto per creare un fantastico effetto visivo. Non fermarti qui, usa la tua immaginazione per dar forma alle tue idee e poi utilizza il potere del codice di Sonic Pi per realizzarle. Quando avrai fatto pratica a sufficienza, abbassa le luci e organizza uno spettacolo visuale per i tuoi amici!
