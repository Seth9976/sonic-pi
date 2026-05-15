A.18 Sound Design - Sintesi Additiva

# Sintesi Additiva

Questo è il primo di una breve serie di articoli su come usare Sonic Pi per il Sound Design. Faremo un rapido tour attraverso un certo numero di tecniche che sono disponibili per creare il tuo suono unico. La prima tecnica che vedremo è chiamata *sintesi additiva". Potrebbe sembrare complicato - ma se riflettiamo brevemente sulle parole, il significato esce fuori subito. Innanzitutto additivo significa una combinazione di cose e secondariamente sintesi significa creare suono. Sintesi additiva significa quindi niente di più complesse che *combinare suoni esistenti per crearne di nuovi*. Questa tecnica di sintesi risale a molto tempo fa - ad esempio gli organi a canne nel medio evo avevano molte canne dai suoni diversi che potevi abilitare o disabilitare con degli appositi blocchi. Rimuovendo il blocco di una certa canna 'la aggiungeva al mix' creando un suono più ricco e complesso. Ora, vediamo come possiamo rimuovere questi blocchi con Sonic Pi.


## Combinazioni Semplici

Cominciamo con il suono più basico che ci sia - l'umile e pura onda sinusoidale:

```
synth :sine, note: :d3
```

Ora, vediamo questo suono combinato con un'onda quadra:

```
synth :sine, note: :d3
synth :square, note: :d3
```

Nota come i due suoni combinati formano un nuovo e più ricco suono. Naturalmente, non dobbiamo fermarci qui, possiamo aggiungere tutti i suoni di qui abbiamo bisogno. Tuttavia, dobbiamo anche stare attenti a quanti suoni aggiungiamo insieme. Proprio come quando combiniamo pitture per creare nuovi colori, aggiungendone troppi otteniamo un paciugo marrone, similmente aggiungendo troppi suoni otterremo un suono impastato.


## Mescolare

Aggiungiamo qualcosa per ottenere un suono più chiaro. Possiamo usare un'onda triangolare in un'ottava più alta (per ottenere quel suono più chiaro) solo però riprodotta con amp `0.4` in modo che aggiunga un extra al suono senza però coprirlo:

```
synth :sine, note: :d3
synth :square, note: :d3
synth :tri, note: :d4, amp: 0.4
```

Ora, prova a creare i tuoi suoni combinando 2 o più synth in ottave e amplificazioni differenti. Nota anche come puoi giocare con le opzioni di ciascun synth per modificare ogni sorgente sonora prima del mix per ottenere ulteriori combinazioni di suoni.


## Scordatura

Fino a ora, combinando i nostri diversi synth abbiamo usato la stessa intonazione o cambiato ottava. Come suonerebbe se non cambiassimo solo le ottave ma invece applicassimo una nota lievemente più alta o più bassa? Proviamo:

```
detune = 0.7
synth :square, note: :e3
synth :square, note: :e3 + detune
```

Se applichiamo una scordatura alle nostre onde quadre di 0.7 note, sentiremo forse qualcosa che non suona corretto o intonato - una 'brutta' nota. Tuttavia, muovendoci più vicini allo 0 suonerà sempre meno stonata man mano che l'intonazione delle 2 onde si avvicina e diventa simile. Prova tu stesso! Modifica l'opzione `detune:` da `0.7` a `0.5` e ascolta il nuovo suono. Prova `0.2`, `0.1`, `0.05`, `0`. Ogni volta che cambi il valore, ascolta e nota come cambia il suono. Nota che una scordatura più bassa come `0.1` produce un suono 'denso' molto bello, con entrambi i suoni lievemente alterati che interagiscono fra loro vengono fuori combinazioni interessanti e spesso sorprendenti.

`:dsaw`


## Ampiezza

Un altro modo con cui possiamo finemente modellare il suono consiste nell'usare differenti envelope e opzioni per ogni innesco dei synth. Ad esempio questo vi permetterà di gestire alcuni aspetti percussivi del suono ed altri aspetti che riguardano la durata nel tempo.

```
detune = 0.1
synth :square, note: :e1, release: 2
synth :square, note: :e1 + detune, amp: 2, release: 2
synth :gnoise, release: 2, amp: 1, cutoff: 60
synth :gnoise, release: 0.5, amp: 1, cutoff: 100
synth :noise, release: 0.2, amp: 1, cutoff: 90
```

Nell'esempio sopra, ho mixato un elemento di rumore percussivo con un brontolio persistente in sottofondo. Questo è stato ottenuto innanzitutto usando 2 synth rumore con valori cutoff intermedi (`90` e `100`) usando tempi di rilascio brevi insieme a rumori con un rilascio più lungo ma con un valore cutoff più basso (il che rende il rumore meno nitido e più rumoroso.)

## Mettiamo tutto insieme

Combiniamo tutte queste tecniche per vedere se possiamo usare la sintesi additiva per creare un suono basico di campana. Ho diviso questo esempio in 4 sezioni. Prima abbiamo la sezione 'hit' che è la parte iniziale del suono della campana - quindi usa un inviluppo corto (e.g. una `release:` di circa `0.1`). Poi abbiamo la lunga sezione di risonanza nella quale uso il suono puro dell'onda sinusoidale. Nota che spesso aumento la nota di circa `12` e `24` che sono il numero di note in una e due ottave. Ho anche inserito un paio di onde sinusoidali basse per dare al suono un po' di bassi e profondità. Infine, ho usato `define` per racchiudere il mio codice in una funzione che posso quindi utilizzare per suonare una melodia. Prova a suonare la tua melodia e anche a incasinare i contenuti della funzione `:bell` finché non crei il tuo suono divertente con cui suonare!

```
define :bell do |n|
  # Triangle waves for the 'hit'
  synth :tri, note: n - 12, release: 0.1
  synth :tri, note: n + 0.1, release: 0.1
  synth :tri, note: n - 0.1, release: 0.1
  synth :tri, note: n, release: 0.2
  # Sine waves for the 'ringing'
  synth :sine, note: n + 24, release: 2
  synth :sine, note: n + 24.1, release: 2
  synth :sine, note: n + 24.2, release: 0.5
  synth :sine, note: n + 11.8, release: 2
  synth :sine, note: n, release: 2
  # Low sine waves for the bass
  synth :sine, note: n - 11.8, release: 2
  synth :sine, note: n - 12, release: 2
end
# Play a melody with our new bell!
bell :e3
sleep 1
bell :c2
sleep 1
bell :d3
sleep 1
bell :g2
```
