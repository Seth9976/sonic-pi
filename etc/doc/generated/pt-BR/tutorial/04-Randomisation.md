4 Randomização

# Randomização (ou Aleatorização)

Obs: randomização vem do inglês *random*, que significa aleatório, ao acaso.

Uma boa forma de tornar sua música mais interessante é usando números aleatórios. O Sonic Pi possui uma excelente funcionalidade para adicionar aleatoriedade à sua musica, mas antes de começar temos que aprender uma verdade chocante: no Sonic Pi *aleatório não é verdadeiramente aleatório*. O que raio quer isto dizer? Bem, vamos ver.

## Repetibilidade

Uma função muito útil para gerar números aleatórios é `rrand` que retorna um valor aleatório entre dois números - um *min* e um *max* (`rrand` é abreviatura de ranged random, *aleatório no intervalo*). Vamos tentar tocar uma nota aleatória:

```
play rrand(50, 95)
```

Oh, uma nota aleatória foi tocada. Foi a nota `83.7527`. Uma bela nota aleatória entre 50 and 95. Opa, espera, eu previ exatamente a nota aleatória que você obteve? Algo estranho está acontecendo aqui. Tente executar o programa novamente. O quê? Escolheu `83.7527` de novo? Isso não pode ser aleatório!

A resposta é que isso não é realmente aleatório, é pseudo-aleatório. O Sonic Pi te dará números que parecem aleatórios de forma repetível. Isto é muito útil para garantir que a música que você criou em uma máquina soe idêntica nas máquinas de qualquer outra pessoa - mesmo que você use aleatoriedade na sua composição.

Claro que, em uma certa composição musical, se toda vez for 'aleatoriamente' escolhido o `83.7527`, não seria nada interessante. Mas não acontece assim. Tente o seguinte:

```
loop do
  play rrand(50, 95)
  sleep 0.5
end 
```

Sim! Finalmente soa a aleatório. Dentro de uma dada *execução*, chamadas subsequentes à função random retornarão valores aleatórios. No entanto, a próxima execução irá produzir exatamente a mesma sequência de valores aleatórios e soará exatamente da mesma forma. É como se todo o código do Sonic Pi voltasse para exatamente o mesmo ponto no tempo, cada vez que o botão Run for pressionado. É o Dia da Marmota (filme: Feitiço do Tempo) da síntese musical!

## Sinos assombrados

Uma linda ilustração de aleatorização em ação é o exemplo 'haunted bells' (sinos assombrados), que faz um loop com a amostra `:perc_bell` com velocidades e tempos de pausa aleatórios entre os sons de sino:

```
loop do
  sample :perc_bell, rate: (rrand 0.125, 1.5)
  sleep rrand(0.2, 2)
end
```

## Corte aleatório

Outro exemplo divertido e randomização é modificar o corte de sintetizadores aleatoriamente. Um bom sintetizador para tentar isto é o emulador `:tb303`:

```
use_synth :tb303
loop do
  play 50, release: 0.1, cutoff: rrand(60, 120)
  sleep 0.125
end
```

## Sementes de números aleatórios

E se você não gostar desta sequência particular de números aleatórios que o Sonic Pi forneceu? Bom, é totalmente possível escolher um ponto inicial diferente, escolhendo uma semente diferente via `use_random_seed`. A semente padrão é 0, então escolha uma semente para ter uma experiência aleatória diferente!

Considere o seguinte:

```
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Cada vez que você executar este código, você ouvirá a mesma sequência de 5 notas. Para obter uma sequência diferente simplesmente mude a semente:

```
use_random_seed 40
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Isto produzirá uma sequência diferente de 5 notas. Mudando a semente e ouvindo o resultado você pode encontrar algo que goste - e quando você compartilhá-lo com outros, eles ouvirão exatamente o que você ouviu também.

Veja algumas outras funções aleatórias úteis.


## choose

Uma atividade muito comum é escolher um item aleatório em uma lista de itens previamente conhecidos. Por exemplo, eu poso querer tocar uma dessas nota: 60, 65 ou 72. Eu posso atingir esse objetivo com `choose` que me deixa escolher um item de uma lista. Primeiro, eu preciso colocar meus números em uma lista, isso é feito envolvendo eles entre colchetes e separando eles com vírgulas  `[60, 65, 72]`. Feito isso basta passá-los para o `choose`:

```
choose([60, 65, 72])
```

Vamos ouvir com o que este som se parece:

```
loop do
  play choose([60, 65, 72])
  sleep 1
end
```

## rrand

Nós já vimos `rrand`, mas vamos recapitular. Ela retorna um número aleatório entre dois valores exclusivamente (ela exclui os limites acima e abaixo). Isso significa que nunca retornará o máximo nem o mínimo - sempre retornará algo entre os dois. O número sempre será um float - número de ponto flutuante, significando que não é um inteiro e sim uma fração. Exemplos de floats retornados por `rrand(20, 110)`:

* 87.5054931640625
* 86.05255126953125
* 61.77825927734375

## rrand_i

Ocasionalmente você desejará um número inteiro, não um float. É ai que entra a `rrand_i`. Ela trabalha de forma similar a ´rrand` exceto que pode retornar os valores mínimo e máximo como potenciais números aleatórios (o que significa que é inclusivo e não exclusivo, em relação aos limites). Exemplos de números retornados por `rrand_i(20,100)` são:

* 88
* 86
* 62

## rand

Isto retornará um número float aleatório, entre 0 (inclusive) e o valor máximo (exclusivo) especificado por você. Por padrão, retornará um valor entre 0 e 1. Portanto é útil para selecionar aleatoriamente valores para `amp:`:

```
loop do
  play 60, amp: rand
  sleep 0.25
end
```

## rand_i

De forma similar à relação entre `rrand_i` e `rrand`, `rand_i` retornará um número inteiro aleatório entre 0 e o valor máximo que você especificou.

## dice (dado)

As vezes você quer simular um laçamento de dados - este é um caso especial de `rrand_i` onde o menor valor é sempre 1. Uma chamada para `dice` requer que você especifique o número de lados do dado. Um dado padrão possui 6 lados, então `dice(6)` agirá de forma muito similar - retornando valores 1, 2, 3, 4, 5 ou 6. No entanto, como nos jogos de RPG, você poderá encontrar um dado de 4 lados, ou um dados de 12 lados ou de 20 lados - talvez até um dado de 120 lados!

## one_in

Por fim, você pode quer simular o lançamento da pontuação máxima de um dado, por exemplo em um dado padrão de 6 lados. `one_in` (um em...) retorna verdadeiro com a probabilidade de um sobre o número de lados do dados. Desta forma, `one_in(6)` retornará verdadeiro com a probabilidade de 1 em 6, ou falso nos outros caso. Valores verdadeiro e falso (true/false) são muito úteis para instruções `if`, que iremos cobrir na próxima seção deste tutorial.

Agora vá e bagunce seu código com alguma aleatoriedade!
