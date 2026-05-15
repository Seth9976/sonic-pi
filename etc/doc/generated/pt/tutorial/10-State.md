10 Time State

# Time State

Frequentemente é útil ter informação *partilhada entre varias threads ou live loops*. Por exemplo, podes querer partilhar a noção da tecla actual, BPM ou até conceitos abstractos como a 'complexidade' actual ( que pode ser interpretada de forma diferente em threads diferentes). Também não queremos a garantia de determinismo existente ao fazermos isto. Ou seja, queremos partilhar código com outros e saber exatamente o que eles vão ouvir ao executa-lo. No final da secção 5.6 deste tutorial nós discutimos brevemente porque é que *não devemos utilizar variáveis para partilhar informação entre threads* devido à possível perca de determinismo (por sua vez devido à condição de corrida).

A solução do Sonic Pi ao problema de lidar com variáveis globais de forma determinística é através de um sistema novo chamado de Time State. Isto pode parecer complexo e difícil (de facto, no Reino Unido, programar com várias threads e memoria partilhada é normalmente um assunto abordado na universidade) No entanto, como poderás ver, tal como tocar a primeira nota, o *Sonic Pi torna incrivelmente fácil a partilha de estado entre múltiplas threads* enquanto mantém os teus programas *thread safe e deterministicos*.

Conhece o `get` e o `set`...
