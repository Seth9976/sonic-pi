A.19 Sound Design - Sintesi Sottrattiva

# Sintesi Sottrattiva

Questo è il secondo di una serie di articoli su come utilizzare Sonic Pi per il sound design. Il mese scorso abbiamo esaminato la sintesi additiva che abbiamo scoperto fosse il semplice atto di suonare più suoni contemporaneamente per creare un nuovo suono combinato. Ad esempio, potremmo combinare sintetizzatori con diversi suoni o anche lo stesso sintetizzatore a diverse intonazioni per costruire un nuovo suono complesso da ingredienti semplici. Questo mese esamineremo una nuova tecnica comunemente chiamata *sintesi sottrattiva* che è semplicemente consiste nel prendere un suono complesso esistente e rimuoverne parti per creare qualcosa di nuovo. Questa è una tecnica comunemente associata al suono dei sintetizzatori analogici degli anni '60 e '70, ma anche alla recente rinascita dei sintetizzatori analogici modulari attraverso standard popolari come Eurorack.

Nonostante sembri una tecnica particolarmente complicata e avanzata, Sonic Pi la rende sorprendentemente semplice, quindi entriamoci subito.

## Segnale Sorgente Complesso

Affinché un suono funzioni bene con la sintesi sottrattiva, in genere deve essere sufficientemente ricco e interessante. Questo non significa che abbiamo bisogno di qualcosa di estremamente complesso - infatti, solo un'onda standard `:square` o `:saw` produrrà:

```
synth :saw, note: :e2, release: 4
```

Nota che questo suono è già piuttosto interessante e contiene molte frequenze diverse sopra `:e2` (il secondo MI su un pianoforte) che si aggiungono per creare il timbro. Se ti sembra che non abbia molto senso, prova a confrontarlo con `:beep`:

```
synth :beep, note: :e2, release: 4
```

Poiché il synth `:beep` è solo un'onda sinusoidale, sentirai un tono molto più puro e solo a `:e2` e nessuno dei suoni acuti e nitidi che hai sentito con `:saw`. Questa vivacità e variazione da un'onda sinusoidale pura costituiscono il materiale con cui possiamo giocare quando utilizziamo la sintesi sottrattiva.

## Filtri

Una volta che abbiamo il nostro segnale sorgente grezzo, il passo successivo è farlo passare attraverso un filtro di qualche tipo che modificherà il suono rimuovendo o riducendone alcune parti. Uno dei filtri più comuni utilizzati per la sintesi sottrattiva è il cosiddetto filtro passa basso. Ciò consentirà il passaggio di tutte le parti basse del suono, ma ridurrà o rimuoverà le parti più alte. Sonic Pi ha un sistema FX potente ma comunque semplice da usare che include un filtro passa basso, chiamato `:lpf`. Giochiamoci un po':

```
with_fx :lpf, cutoff: 100 do
  synth :saw, note: :e2, release: 4
end
```

Se ascolti attentamente, sentirai come un po' di quel brusio e quella nitidezza sono state rimosse. Infatti, tutte le frequenze nel suono sopra la nota `100` sono state ridotte o rimosse e solo quelle sotto sono ancora presenti nel suono. Prova a cambiare quel "cutoff:" punta alle note più basse, diciamo dì "70" e poi "50" e confronta i suoni.

Naturalmente, `:lpf` non è l'unico filtro che puoi usare per manipolare il segnale sorgente. Un altro effetto importante è il filtro passa alto denominato `:hpf` in Sonic Pi. Questo fa l'opposto di `:lpf` in quanto lascia passare le parti alte del suono e taglia le parti basse.

```
with_fx :hpf, cutoff: 90 do
  synth :saw, note: :e2, release: 4
end
```

Nota come questo suona con molto più ronzio e in modo roco ora che tutti i suoni a bassa frequenza sono stati rimossi. Prova a cambiare il valore di cutoff: nota come valori più bassi lasciano passare più parti dei bassi originali del segnale sorgente e valori più alti suonano sempre più metallici e silenziosi.

## Filtro Passa Basso

![Varying amounts of low pass filtering](../../../etc/doc/images/tutorial/articles/A.19-subtractive-synthesis/subtractive-synthesis-waveforms.png)

`:prophet`


## Modulazione Filtri

Finora abbiamo appena prodotto suoni abbastanza statici. In altre parole, il suono non cambia in alcun modo per tutta la sua durata. Spesso potresti volere un po' di movimento nel suono per dare vita al timbro. Un modo per ottenere ciò è tramite la modulazione del filtro - modificando le opzioni del filtro nel tempo. Fortunatamente Sonic Pi ti offre potenti strumenti per manipolare le opzioni di un FX nel tempo. Ad esempio, puoi impostare un tempo di transizione per ciascuna opzione modulabile per specificare quanto tempo dovrebbe impiegare il valore corrente per scorrere linearmente fino al valore di destinazione:

```
with_fx :lpf, cutoff: 50 do |fx|
  control fx, cutoff_slide: 3, cutoff: 130
  synth :prophet, note: :e2, sustain: 3.5
end
```

Diamo una rapida occhiata a cosa sta succedendo qui. Innanzitutto iniziamo un blocco FX `:lpf` normale con un `cutoff:` iniziale basso di un `50`. Tuttavia, la prima riga termina anche con uno strano `|fx|` alla fine. Questa è una parte opzionale della sintassi `with_fx` che ti permette di denominare e controllare direttamente il synth FX in esecuzione. La riga 2 fa esattamente questo e controlla l'FX per impostare opzione `cutoff_slide:` a 3 e il nuovo valore di destinazione `cutoff:` a `130`. L'FX ora inizierà a muovere il valore di `cutoff:` da `50` a `130` lungo un periodo di 3 battiti. Infine attiviamo anche un sintetizzatore del segnale sorgente in modo da poter ascoltare l'effetto del filtro passa basso modulato.


## Mettiamo tutto insieme

Questo è solo un assaggio molto semplice di ciò che è possibile ottenere quando si utilizzano i filtri per modificare e cambiare un suono sorgente. Prova a giocare con i numerosi effetti integrati di Sonic Pi per vedere quali suoni divertenti puoi progettare. Se il tuo suono sembra troppo statico, ricorda che puoi iniziare a modulare le opzioni per creare un po' di movimento.

Concludiamo progettando una funzione che riprodurrà un nuovo suono creato con sintesi sottrattiva. Guarda se riesci a capire cosa sta succedendo qui - e per gli utenti avanzati di Sonic Pi - guardate se riuscite a capire perché ho racchiuso tutto in una chiamata a `at` (per favore inviate le risposte a @samaaron su Twitter).

```
define :subt_synth do |note, sus|
  at do
    with_fx :lpf, cutoff: 40, amp: 2 do |fx|
      control fx, cutoff_slide: 6, cutoff: 100
      synth :prophet, note: note, sustain: sus
    end
    with_fx :hpf, cutoff_slide: 0.01 do |fx|
      synth :dsaw, note: note + 12, sustain: sus
      (sus * 8).times do
        control fx, cutoff: rrand(70, 110)
        sleep 0.125
      end
    end
  end
end
subt_synth :e1, 8
sleep 8
subt_synth :e1 - 4, 8
```
