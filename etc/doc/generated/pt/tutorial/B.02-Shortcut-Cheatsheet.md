10.2 Folha de ajuda de atalhos

# Folha de ajuda de atalhos

The following is a summary of the main Emacs Live shortcuts available within Sonic Pi. Please see Section B.1 for motivation and background.

## Convenções

Nesta lista, usamos a seguinte convenção (onde *Meta* ou é o *Alt* em Windows/Linux ou o *Cmd* no Mac):

* `C-a` significa carregar na tecla *Control* e depois carrega a tecla *a* estando as duas pressionadas ao mesmo tempo, depois libertando.
* `M-r` significa pressionar a tecla *Meta* e depois pressionar a tecla *r* ao mesmo tempo, depois libertando.
* `S-M-z` significa presionar a tecla *Shift*, depois a tecla *Meta*, e finalmente a tecla *z* todas ao mesmo tempo, depois libertando.
* `C-M-f` significa premir a tecla *Control*, e depois premir a tecla *Meta*, finalmente a tecla *f* todas ao mesmo tempo, depois libertando.

## Manipulação da Aplicação principal

* `M-r` - Correr código
* `M-s` - Parar código
* `M-i`- Comutar o sistema de ajuda
* `M-p`- Comutar preferencias
* `M-{` - Mudar para a buffer da esquerda
* `M-}` - Mudar para a buffer da direita
* `S-M-0` - Switch to buffer 0
* `S-M-1` - Switch to buffer 1
* ...
* `S-M-9` - Switch to buffer 9
* `M-+` - Aumentar tamanho do texto da buffer corrente
* `M--` - Reduzir o tamanho de texto na corrente buffer


## Selecionar/copiar/colar

* `M-a` - Selecionar tudo
* `M-c`- Copiar selecção para colar na buffer
* `M-]` - Copiar seleção para colar no buffer
* `M-x` - Cortar seleção para colar na buffer
* `C-]` - Cortar seleção para a colar na buffer
* `C-k`- Cortar até ao fim da linha
* `M-v`- Colar para o editor
* `C-y` - Colar para o editor
* `C-SPACE` - Colocar marca. A navegação irá agora manipular a região sublinhada. Usar `C-g`para escapar.

## Manipulação de texto

* `M-m` - Alinhar todo o texto
* `Tab`- Alinha a corrente linha ou seleção (ou escolhe autocompletion)
* `C-l` - Centra o editor
* `M-/` - Comuta os comentários da linha actual ou seleção
* `C-t` - Transpõe/troca caracteres
* `M-u`- Converte a próxima palavra (ou selecção) para maiúsculas.
* `M-l` - Converte a próxima palavra (ou selecção) para minúsculas.

## Navegação

* `C-a` - Mover para o inicio da linha
* `C-e` - Mover para o final da linha
* `C-p` - Mover para a linha anterior
* `C-n` - Mover para a próxima linha
* `C-f` - Mover para a frente um caractere
* `C-b` - Mover para trás um caractere
* `M-f` - Mover para a frente uma palavra
* `M-b`- Mover para trás uma palavra
* `C-M-n` - Mover uma linha ou selecção para baixo
* `C-M-p` - Mover linha ou selecção para cima
* `S-M-u` - Mover para cima 10 linhas
* `S-M-d`- Mover para baixo 10 linhas
* `M-<` - Mover para o inicio da buffer
* `M->` - Mover para o final da buffer

## Remoção

* `C-h` - Remove o caractere anterior
* `C-d` - Remove o proximo caractere

## Características avançadas do editor

* `C-i` - Mostra documentação sobre a palavra por baixo do cursor
* `M-z` - Undo
* `S-M-z` - Refazer
* `C-g` - Escapar
* `S-M-f` - comutar ecrã total
* `S-M-b` - Comutar visibilidade dos botões
* `S-M-l` - Comuta visibilidade do registo
* `S-M-m` - Comuta entre o modo claro/escuro
* `S-M-s`- Salva o conteúdo de uma buffer para um ficheiro
* `S-M-o` - Carrega o conteúdo de uma buffer para um ficheiro
