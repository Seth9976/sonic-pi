A.1 Dicas para o Sonic Pi

# As cinco melhores dicas

## 1. Não existem erros

A coisa mais importante lição a aprender com o Sonic Pi é que realmente não existem erros. A melhor maneira é tentar e tentar. Tenta coisas diferentes, pára de te preocupar se o teu código soa bem ou não e começa a experimentar com diferentes synths, notas, efeitos e opções o máximo possível. Descobrirás muitas coisas que te farão rir porque soam mal e algumas pérolas que soam realmente bem. Simplesmente retira o que não gostas e fica com as que gostas. Quantos mais 'erros' te permitires mais rápido aprenderás e descobrirás o teu som pessoal no código.


## 2. Usa os efeitos

Digamos que já dominas o básico do Sonic Pi de fazer sons com `sample`, `play`? O que se segue? Sabias que o Sonic Pi suporta mais de 27 efeitos de estúdio para mudar o som do teu código? Efeitos são como filtros de imagens bonitos nos programas de desenho excepto que em vez de desfocar ou tornar algo a preto e branco, podes acrescentar reverb, distorção e eco ao teu som. Pensa que é como ligar o cabo de uma guitarra a um pedal de efeitos à tua escolha e depois ao amplificador. Felizmente, o Sonic Pi torna o uso de efeitos muito fácil e não requer cabos! Tudo o que tens que fazer é escolher que secção do código queres adicionar o efeito e embrulhares com o código do efeito. Vamos ver um exemplo. Imagina que tens o seguinte código:

```
sample :loop_garzul
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Se quiseres acrescentar um efeito ao sample `:loop_garzul`, apenas tens que o colocar dentro de um bloco `with_fx` assim:

```
with_fx :flanger do
  sample :loop_garzul
end
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Agora, se quiseres adicionar um efeito à bass drum, vai e embrulha isso com o `with_fx` também:

```
with_fx :flanger do
  sample :loop_garzul
end
with_fx :echo do
  16.times do
    sample :bd_haus
    sleep 0.5
  end
end
```

Lembra-te, podes embrulhar *qualquer* código com o `with_fx` e qualquer som criado passará pelo efeito.


## 3. Paramatriza os teus synths

De forma a realmente descobrires o teu som no código em breve quererás saber como modificar e controlar os synth e efeitos. Por exemplo, poderás querer mudar a duração de uma nota, adicionar mais reverb, ou mudar o tempo entre os ecos. Felizmente, o Sonic Pi dá-te um incrível nível de controlo para fazer isso com uma coisa especial chamada parâmetros opcionais ou opts como abreviatura. Vamos dar uma vista de olhos. Copia este código para o espaço de trabalho e carrega run:

```
sample :guit_em9
```

Ooh, um bonito som de guitarra! Agora, vamos começar a brincar com ele. Que tal mudar a sua rate?

```
sample :guit_em9, rate: 0.5
```

Hey, o que é essa `rate: 0.5` que acrescentei no final? É chamada um opt. Todos os Synths do Sonic Py e efeitos suportam-no e existem muitos para brincar com eles. Eles também estão disponíveis para efeitos. Tenta isto:

```
with_fx :flanger, feedback: 0.6 do
  sample :guit_em9
end
```

Agora, tenta aumentar o feedback para 1 para ouvires alguns sons malucos! Lê a documentação para todos os detalhes de todas as opts disponíveis para ti.


## 5. Código ao vivo

A melhor maneira de experimentar e explorar o Sonic Pi é codificares ao vivo. Isto permite que comeces um código e o mudes e afines enquanto esta a ser tocado. Por exemplo, se não sabes o que o parâmetro cutoff faz a um sample, brinca com isso. Vamos experimentar! Copia este código para um espaço de trabalho do Sonic Pi:

```
live_loop :experiment do
  sample :loop_amen, cutoff: 70
  sleep 1.75
end
```

Agora, carrega run e ouvirás um drum break ligeiramente abafado. Agora, muda o valor de `cutoff:` para `80` e carrega outra vez em run. Consegues ouvir a diferença? Experimenta `90`, `100`, `110`...

Once you get the hang of using `live_loop`s you'll not turn back. Whenever I do a live coding gig I rely on `live_loop` as much as a drummer relies on their sticks. For more information about live coding check out Section 9 of the built-in tutorial.

## 5. Surf the random streams

Finally, one thing I love doing is cheating by getting Sonic Pi to compose things for me. A really great way to do this is using randomisation. It might sound complicated but it really isn't. Let's take a look. Copy this into a spare workspace:

```
live_loop :rand_surfer do
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Now, when you play this, you'll hear a constant stream of random notes from the scale `:e2 :minor_pentatonic` played with the `:dsaw` synth. "Wait, wait! That's not a melody", I hear you shout! Well, here's the first part of the magic trick. Every time we go round the `live_loop` we can tell Sonic Pi to reset the random stream to a known point. This is a bit like going back in time in the TARDIS with the Doctor to a particular point in time and space. Let's try it - add the line `use_random_seed 1` to the `live_loop`:

```
live_loop :rand_surfer do
  use_random_seed 1
  use_synth :dsaw
  notes = (scale :e2, :minor_pentatonic, num_octaves: 2)
  16.times do
    play notes.choose, release: 0.1, cutoff: rrand(70, 120)
    sleep 0.125
  end
end
```

Now, every time the `live_loop` loops around, the random stream is reset. This means it chooses the same 16 notes every time. Hey presto! An instant melody. Now, here's the really exciting bit. Change the seed value from `1` to another number. Say `4923`. Wow! Another melody! So, just by changing one number (the random seed), you can explore as many melodic combinations as you can imagine! Now, that's the magic of code.
