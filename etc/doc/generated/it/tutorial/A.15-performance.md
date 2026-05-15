A.15 Cinque tecniche di live coding

# Cinque tecniche di live coding

Nella guida di questo mese ti spiegheremo che Sonic Pi può essere utilizzato come un vero strumento musicale. Quindi dobbiamo cominciare a pensare al codice in un modo completamente diverso. I live coder pensano al codice più o meno nello stesso modo in cui un violinista pensa al suo archetto. Infatti, come il violinista può usare diverse tecniche esecutive per creare suoni diversi (movimento lento e colpetti veloci per esempio), noi esploreremo qui 5 regole di base del live coding che si possono eseguire su Sonic Pi. Alla fine di questo articolo, tu sarai capace di eseguire la tua musica per mezzo del live coding.

## 1. Memorizza i tasti di scelta rapida

Uno dei primi consigli da seguire a proposito del live coding, è quello di cominciare ad usare le combinazioni di tasti (shortcut). Ad esempio, invece di perdere tempo cercando il mouse, muoverlo sul bottone Run e cliccarlo, puoi semplicemente premere `alt` e `r` contemporaneamente. Oltre ad essere molto più veloce, le tue dita saranno già sulla tastiera pronte a digitare il prossimo comando. Puoi vedere ognuna delle combinazioni per le funzioni principali andando con il cursore del mouse sopra il pulsante relativo alla funzione stessa. Oppure, al paragrafo 10.2 del tutorial puoi trovare l'intera lista.

Mentre stai suonando, una cosa divertente da fare è dare un po' di enfasi al movimento delle braccia quando effettui una combinazione di tasti. Ad esempio, è sempre buona cosa comunicare agli spettatori quando stai per introdurre un cambiamento, quindi amplifica il gesto quando digiti `alt-r`, al pari di un chitarrista che dà una potente pennata.

## 2. Stratifica manualmente i tuoi suoni

Ora puoi far partire codice istantaneamente con la tastiera, puoi usare subito questa abilità per la nostra seconda tecnica che consiste nello stratificare suoni in modo manuale. Invece di 'comporre' usando molte chiamate a `play` e `sample` separate da `sleep` avremo un'unica chiamata a `play` che avvieremo automaticamente usando `alt-r`. Proviamo! Inserisci questo codice in un buffer vuoto:

```
synth :tb303, note: :e2 - 0, release: 12, cutoff: 90
```

Ora, premi `Avvia` e, mentre il suono viene riprodotto, modifica il codice in modo da eliminare quattro note cambiando il seguente codice:


```
synth :tb303, note: :e2 - 4, release: 12, cutoff: 90
```

Ora, premi `Avvia` di nuovo per sentire entrambi i suoni nello stesso momento. Questo perché il pulsante `Avvia` di Sonic Pi non aspetta di finire di eseguire il codice già avviato ma, invece, fa partire il codice già avviato nello stesso momento. Questo vuol dire che puoi stratificare molti suoni semplicemente e manualmente con piccole modifiche tra un avvio e l'altro. Per esempio, prova a cambiare `note:` e `cutoff:` e a rilanciare il codice.


Puoi usare questa tecnica con campioni lunghi. Per esempio:

```
sample :ambi_lunar_land, rate: 1
```

Prova facendo partire il campione e, poi, abbassa progressivamente il valore di `rate:` da `1` a `0.5` a `0.25` fino a `0.125` premendo continuamente `Avvia`. Prova anche con valori negativi come `-0.5`. Stratifica i suoni e vedi dove puoi arrivare. Prova anche ad aggiungere degli effetti.

Quando ti esibisci, lavorare con linee di codice molto semplice può aiutare il pubblico che non ha mai visto Sonic Pi a seguire quello che stai facendo riuscendo a collegare quanto stati scrivendo con quello che sentono.


## 3. Controllare i live loop

Quando si lavora con della musica più ritmata, può risultare spesso difficile sincronizzare tutto e mantenere un buon tempo. In questi casi è preferibile utilizzare un 'live_loop'. Oltre a garantire la ripetizione al tuo codice, ti dà la possibilità di modificarlo, aggiornandolo per la prossima iterazione. Inoltre, allo stesso tempo altri `live_loop` possono essere eseguiti, il che significa che è possibile sovrapporli fra loro, o a istruzioni manuali. Dai un'occhiata alla sezione 9.2 del tutorial per ulteriori informazioni sull'utilizzo dei live loop.

Quando esegui un'esecuzione dal vivo, ricordarti di utilizzare nel `live_loop` l'opzione `sync:` in modo da rimediare ad eventuali errori di runtime che possono interrompere l'esecuzione in loop del tuo brano. Se hai già introdotto un `sync:` che punta a un altro valido `live_loop`, allora si può rapidamente correggere l'errore, eseguendo nuovamente il codice e ripartire senza perdere il tempo.

## 4. Usare il mixer master

Uno dei segreti meglio custoditi di Sonic Pi è che esiste un mixer principale attraverso il quale confluiscono tutti i suoni. Questo mixer ha di fabbrica sia un filtro passa-basso che un filtro passa-alto, quindi è possibile operare facilmente delle modifiche globali al suono in uscita. Le funzionalità del mixer principale sono accessibili tramite la funzione `set_mixer_control!`. Ad esempio, mentre del codice è in esecuzione e sta suonando, basta inserire la funzione in un buffer vuoto ed eseguire  `Run`:

` set_mixer_control! lpf: 50 `

Dopo l'esecuzione di questo codice, verrà attivato un filtro passa basso, ottenendo un suono finale più "ovattato". Nota bene che i nuovi parametri del mixer principale verranno mantenuti finché non deciderai di modificarli nuovamente. Tuttavia, è possibile reimpostare in qualsiasi momento il mixer al suo stato predefinito con la funzione `reset_mixer!`. Alcune delle opzioni attualmente supportate sono: `pre_amp:`, `lpf:` `hpf:`, e `amp:`. Per l'elenco completo, dai un'occhiata alla documentazione relativa a `set_mixer_control!`.

Utilizza l'opzione ' * _slide' del mixer per far scorrere uno o molti valori delle opzioni nel tempo. Ad esempio, per far scorrere lentamente i valori del filtro passa-basso del mixer dal valore corrente a 30, prova a digitare il codice seguente:

```
set_mixer_control! lpf_slide: 16, lpf: 30
```

Puoi scorrere velocemente a un valore più alto così:

```
set_mixer_control! lpf_slide: 1, lpf: 130
```

Quanto ti esibisci può tornare utile tenere un buffer disponibile per lavorare con il mixer in questo modo.

## 5. Pratica

La tecnica più importante di live coding è fare pratica. La qualità più comune tra i musicisti professionisti di qualsiasi tipo è che fanno pratica con il loro strumento spesso per molte ore al giorno. Esercitarsi è importante per un live coder tanto quanto per un chitarrista. Fare esercizio permette alle tue dita di memorizzare alcuni pattern e modifiche comuni per cui puoi digitare e lavorare in modo più fluido. L'esercizio ti dà anche l'opportunità di esplorare nuovi suoni e codici.

Quando ti esibisci, scoprirai che più esercizio fai, più sarà semplice rilassarsi durante il concerto. La pratica ti permetterà di avere un catalogo di esperienze da cui prendere spunto. Questo ti permette di capire che tipo di modifiche saranno interessanti e quali funzioneranno bene con i suoni che stai utilizzando in quel momento.

## Mettiamo tutto insieme

Questo mese, invece di presentare un esempio riassuntivo delle cose già fatte, iniziamo con una sfida. Prova a vedere se riesci a esercitarti tutti i giorni della settimana con gli esercizi che ti fornirò. Ad esempio, un giorno fai pratica con i trigger manuali, il successivo crea quale 'live_loop', quelo ancora dopo esercitati con il mixer principale, e così via. Poi ripeti tutto da capo. Non preoccuparti se ti senti lento/a o goffo/a in un primo momento - basta che ti eserciti ogni giorno e prima di rendertene conto sarai davanti a un pubblico vero e suonerai come una rockstar, o meglio come una live coding star.
