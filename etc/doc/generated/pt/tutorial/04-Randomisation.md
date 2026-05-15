4 Randomização

# Randomização

Uma boa maneira de adicionar algum interesse à musica é usando números aleatórios. Sonic Pi apresenta uma excelente funcionalidade em adicionar aleatoriedade à sua musica, mas antes de começar temos que aprender a verdade chocante: no Sonic Pi *random não é verdadeiramente random*. O que raio quer isto dizer? Bem, vamos ver.

## Repetibilidade

Uma função verdadeiramente útil para gerar números aleatórios é `rrand` que devolve um número aleatório entre dois números - um *min* e um *max* (`rrand` é abreviatura de ranged random). Vamos tentar tocar uma nota aleatória:

```
play rrand(50, 95)
```

Ooh, tocou uma nota aleatória. Tocou a nota `83.7527`. Um simpática nota entre 50 e 95. Espera, previ a mesma nota que tu? Algo estranho está a acontecer. Tenta correr o código novamente. O quê? Escolheu `83.7527` novamente? Isto não pode ser aleatório!

A resposta é que não é verdadeiramente aleatório, é pseudo-aleatório. Sonic Pi gera números pseudo-aleatórios de uma maneira repetível. Isto é bastante útil para assegurar que a musica que crias na tua maquina soa idêntica na maquina de qualquer pessoa - mesmo que use aleatoriedade na tua composição.

Claro que, numa dada peça de musica, se 'aleatoriamente' escolhe `83.7527`todas as vezes, não seria muito interessante. No entanto, não o faz. Experimente o seguinte:

```
loop do
  play rrand(50, 95)
  sleep 0.5
end 
```

Sim! Finalmente soa a aleatório. Numa dada *run* subsequentes chamadas à função random devolve valores aleatórios. No entanto, a próxima corrida ira produzir exactamente a mesma sequência de valores aleatórios e soa exactamente igual. É como se todo o código do Sonic Pi regressa-se no tempo exactamente ao mesmo ponto cada vez que se pressiona o botão Run. É o dia do Groundhog da síntese musical!

## Sinos amaldiçoados

Uma bonita ilustração da aleatoriedade em acção é o exemplo dos sinos amaldiçoados em que o loop do sample `:perc_bell` é tocado a uma velocidade e tempo de descanço aleatorio entre os sons de sinos.

```
loop do
  sample :perc_bell, rate: (rrand 0.125, 1.5)
  sleep rrand(0.2, 2)
end
```

## Cutoff aleatório

Outro exemplo giro de aleatoriedade é a modificação do cutoff de um sintetizador de forma aleatória. Um bom sintetizador para experimentar isto é o emulador `:tb303`:

```
use_synth :tb303
loop do
  play 50, release: 0.1, cutoff: rrand(60, 120)
  sleep 0.125
end
```

## Sementes para gerar números aleatórios

Então e se não gostares desta sequência em particular de números aleatórios que o Sonic Pi fornece? É totalmente possível escolher um diferente ponto de partida usando `use_random_seed` . A semente padrão usada para gerar números aleatórios é 0, escolhe uma semente diferente para uma diferente experiência aleatória!

Considera o seguinte:

```
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Cada vez que corres este código, ouvirás a mesma sequência de 5 notas. Para ter uma diferente sequência simplesmente altera a semente:

```
use_random_seed 40
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Isto irá produzir uma sequência diferente de 5 notas. Ao alterar a semente e ouvindo o resultado poderás encontrar algo que gostes - e depois partilhar com outros, eles ouviram exactamente o que ouviste.

Vamos ver algumas outras funções aleatórias úteis.


## escolhe

Algo bastante comum de se fazer é escolher um item aleatoriamente de uma determinada lista de itens conhecidos. Por exemplo, posso querer tocar uma nota da seguinte lista: 60, 65 ou 72. Posso conseguir isso com `choose`que me deixa escolher um item da lista. Primeiro coloco os meus números numa lista o que é feito envolvendo-os em parênteses rectos separados por virgulas: `[60, 65, 72]`. A seguir só temos que os passar ao `choose`:

```
choose([60, 65, 72])
```

Vamos ouvir como soa:

```
loop do
  play choose([60, 65, 72])
  sleep 1
end
```

## rrand

Já vimos `rrand`, mas voltando a ela. Ela devolve um número aleatório entre dois números exclusivamente. Isso significa que numa irá fornecer o máximo ou o mínimo - mas sim sempre algo entre os dois. O número será sempre um número flutuante - que não é um numero inteiro mas sim a fracção de um numero. Exemplo de números flutuantes devolvidos por `rrand(20,110)`:

* 87.5054931640625
* 86.05255126953125
* 61.77825927734375

## rrand_i

Ocasionalmente queremos um número inteiro e não um flutuante. É ai que a `rrand_i` nos vem ajudar. Trabalha da forma similar a ´rrand`excepto que pode devolver o min e o max como potencial número aleatório (o que significa que é inclusivo e não exclusivo nos limites). Exemplos de números devolvidos por `rrand_i(20,100)` são:

* 88
* 86
* 62

## rand

Isto devolve um número aleatório flutuante entre 0 (inclusive) e um valor máximo especificado (exclusivo). Por padrão irá devolver um valor entre 0 e 1. É assim útil para escolher valores aleatórios `amp:`:

```
loop do
  play 60, amp: rand
  sleep 0.25
end
```

## rand_i

Similarmente à relação entre `rrand_i` e `rrand`, `rand_i` irá devolver um número aleatório inteiro entre 0 e o valor máximo que especificaste.

## Dado

Às vezes queres emular o lançamento de um dado - isto é um caso especial de `rrand_i` em que o valor mais baixo é 1. Chamar `dice` requer que especifiques o número de lados do dado. Um dado normal tem 6 lados, assim `dice(6)` irá se comportar de forma semelhante - devolvendo os valores 1, 2, 3, 4, 5 ou 6. No entanto, tal como nos jogos de role play, poderás encontrar dados de 4 lados, ou 12 ou 20, ou até mesmo uma dado de 120 lados.

## one_in

Finalmente poderás querer emular o lançamento de uma pontuação máxima num dado tal como um 6 num dado normal. `one_in` devolve true com a probabilidade de um sobre o número de lados do dado. Assim `one_in(6)` irá devolver true com a probabilidade de 1 em 6 ou false em caso contrario. True e false são valores muito úteis nas declarações condicionais `if`, que serão descritas nas seguintes secções deste tutorial.

Agora vai e enche o teu código de aleatoriedade!
