A.17 Allungare i campioni

# Allungare i campioni

Quando le persone scoprono Sonic Pi, una delle prime cose che impara è quanto sia semplice riprodurre suoni pre-registrati usando la funzione `sample`. Ad esempio, puoi suonare un loop di tamburo industriale, ascoltare il suono di un coro o anche ascoltare un graffio di vinile tutto attraverso una singola riga di codice. Tuttavia, molte persone non si rendono conto che è possibile effettivamente variare la velocità, il campione viene riprodotto per alcuni effetti potenti e un nuovo livello di controllo sui suoni registrati. Quindi, scarica una copia di Sonic Pi e iniziamo a giocare con  alcuni campioni!

## Rallentare i campioni (samples)

Per modificare la frequenza di riproduzione di un campione dobbiamo utilizzare il `rate:` opt:

```
sample :guit_em9, rate: 1
```
    
Se si specifica un `rate:` di `1` allora il campione viene riprodotto alla velocità normale. Se vogliamo riprodurlo ad una velocità dimezzata, usiamo semplicemente una `rate:` di `0.5`:


```
sample :guit_em9, rate: 0.5
```
    
Tieni conto che questo ha un duplice effetto sul suono. In primo luogo il campione suonerà più basso, più grave e in secondo luogo ci metterà il doppio ad essere riprodotto (dai un'occhiata alla barra laterale per una spiegazione più approfondita). Possiamo addirittura scegliere anche dei valori ancora più bassi avvicinandoci allo '0'. Per esempio '0.25' è un quarto della velocità originale, '0.1' è un decimo e così via. Prova a suonare i campioni utilizzando numeri bassi e osserva come puoi trasformare il suono in una specie di basso ruggito.

## Accelerare i campioni

Oltre a prolungare ed abbassare i suoni mediante l'abbassamento del valore di rate, possiamo utilizzare dei valori più alti per accorciare e rendere più acuti i suoni. Proviamo a riprodurre un loop di batteria stavolta. Per prima cosa, prova ad ascoltare come suona alla velocità normale: '1':

```
sample :loop_amen, rate: 1
```


Ora acceleriamoli un pò:

```
sample :loop_amen, rate: 1.5
```
    
Ha! Ci siamo appena spostati di genere musicale dalla techno vecchia scuola alla jungle. Nota come l'intonazione di ogni colpo di batteria è più alta e come l'intera sezione ritmica accelera. Ora, prova rate ancora più alte e verifica quanto alto e corto puoi rendere il loop di batteria. Ad esempio, se usi un rate di `100`, il loop di batteria diventa un click!

## Retromarcia

Ora, sono sicuro che molti di voi stanno pensando la stessa cosa adesso ... "cosa succede se si utilizza un numero negativo per il rate?". Grande domanda! Pensiamo a questo per un momento. Se il nostro `rate:` opt significa la velocità con cui viene riprodotto il campione, `1` è la velocità normale,` 2` è doppia velocità, `0.5` è mezza velocità,` -1` deve significare all'indietro! Proviamolo su un rullante. Innanzitutto, riprodurlo al ritmo normale:

```
sample :elec_filt_snare, rate: 1
```
    
Adesso fallo andare all’incontrario:

```
sample :elec_filt_snare, rate: -1
```
    
Naturalmente, puoi riprodurlo al contrario al doppio della velocità con il rate a `-2` oppure al contrario a mezza velocità con un rate a `-0.5`. Ora, prova a giocare con differenti rate negativi e divertiti. E' particolarmente divertente con il sample `:misc_burp`!


## Campioni, Frequenza e Intonazione

Uno degli effetti del cambio di frequenza sui campioni è che frequenze più veloci portano i campioni a suonare più acuti e frequenze più lente portano i campioni a suonare più gravi in intonazione. Un altro contesto in cui potresti aver sentito questo effetto nella vita di tutti i giorni è quando passi in bicicletta o in macchina vicino al cicalino di un attraversamento pedonale - quando ti stai avvicinando l'intonazione è più alta di quando ti stai allontanando - il così detto effetto Doppler. Perché accade?

Consideriamo un semplice beep che è rappresentato da un'onda sinusoidale. Se usiamo un oscilloscopio per tracciare un beep, vedremmo qualcosa tipo la Figura A. Se tracciamo un beep un ottava più in alto, vedremo la Figura B e un ottava più in basso sarà come nella Figura C. Nota che le forme d'onda di note più alte sono più compatte mentre le forme d'onda di note più basse sono più dilatate.

Il campione di un beep non è altro che un insieme di molti numeri (coordinate, x,y) le quali quando tracciate in un grafico ridisegneranno le curve originali. Vedi la figura D in cui ogni cerchio rappresenta una coordinata. Per riconvertire le coordinate in audio, il computer valuta ogni valore di x e lo manda manda il corrispondente valore di y ai diffusori. Il trucco qui sta nel fatto che la frequenza alla quale il computer lavora i numeri x non deve essere la stessa frequenza alla quale sono stati registrati. In altre parole, lo spazio (che rappresenta un intervallo di tempo) fra ogni cerchio può essere allungato o compresso. Così, se il computer processa i valori di x più velocemente della frequenza originale, avrà l'effetto di schiacciare i cerchi più vicini tra loro, il che risulterà in un segnale acustico più acuto. Causerà inoltre un beep più corto in quanto processeremo tutti i cerchi più velocemente. Questo è mostrato nella figura E.

Infine, un'ultima cosa da sapere è che un matematico chiamato Fourier ha provato che ogni suono è in effetti costituito da moltissime onde sinusoidali combinate insieme. Quindi, quando comprimiamo e allunghiamo qualunque suono registrato, stiamo in realtà comprimendo e allungando molte onde sinusoidali simultaneamente in questo modo.

## Alterazione dell'Intonazione

Come abbiamo visto, una frequenza più veloce porterà a un suono più alto in intonazione e una frequenza più lenta porterà a suono più basso in intonazione. Un trucchetto molto semplice e veloce consiste nel sapere che a un raddoppio della frequenza corrisponde un'intonazione più alta di un'ottava e, inversamente, il dimezzamento della frequenza corrisponde un'intonazione più bassa di un'ottava. Questo significa che per i sample melodici, riprodurli su sè stessi al doppio o alla metà della loro frequenza li fa suonare piuttosto bene:

```
sample :bass_trance_c, rate: 1
sample :bass_trance_c, rate: 2
sample :bass_trance_c, rate: 0.5
```
    
Tuttavia, come dobbiamo procedere se vogliamo alterare la frequenza in modo che alzi di un semitono (una nota più avanti sul piano)? Sonic Pi rende questo molto semplice attraverso l'opzione `rpitch:`:

```
sample :bass_trance_c
sample :bass_trance_c, rpitch: 3
sample :bass_trance_c, rpitch: 7
```
    
Se dai un'occhiata al registro eventi sulla destra, noterai che un `rpitch:` di `3` corrisponde in effetti a una frequenza di `1.1892` mentre `rpitch:` di `7` corrisponde a una frequenza di `1.4983`. Infine possiamo anche combinare le opzioni `rate:` e `rpitch:`:

```
sample :ambi_choir, rate: 0.25, rpitch: 3
sleep 3
sample :ambi_choir, rate: 0.25, rpitch: 5
sleep 2
sample :ambi_choir, rate: 0.25, rpitch: 6
sleep 1
sample :ambi_choir, rate: 0.25, rpitch: 1
```
    

## Mettiamo tutto insieme

Diamo un'occhiata a un semplice brano che combina queste idee. Copia in un buffer Sonic Pi vuoto, premi play, ascolta per un po' e poi usalo come punto di partenza per il tuo brano personale. Guarda quanto è divertente manipolare la frequenza di riproduzione dei sample. Come esercizio aggiuntivo prova a registrare dei tuoi suoni a giocare con le frequenze per vedere che razza di suoni selvaggi puoi creare.

```
live_loop :beats do
  sample :guit_em9, rate: [0.25, 0.5, -1].choose, amp: 2
  sample :loop_garzul, rate: [0.5, 1].choose
  sleep 8
end
 
live_loop :melody do
  oct = [-1, 1, 2].choose * 12
  with_fx :reverb, amp: 2 do
    16.times do
      n = (scale 0, :minor_pentatonic).choose
      sample :bass_voxy_hit_c, rpitch: n + 4 + oct
      sleep 0.125
    end
  end
end
```







