A.5 Basse acide

# Acid Bass

Il est impossible de parcourir l'histoire de la musique de danse électronique sans voir l'énorme impact du minuscule synthétiseur Roland TB-303. C'est la sauce secrète derrière le son de basse acide original. Ces riffs de basse TB-303 qui grincent et étouffent peuvent être entendus depuis la première scène de Chicago House jusqu'aux artistes électroniques plus récents tels que Plastikman, Squarepusher et Aphex Twin.

Il est intéressant de noter que Roland n'a jamais eu l'intention d'utiliser le TB-303 dans la musique de danse. Il a été créé à l'origine pour aider les guitaristes à s'exercer. Ils imaginaient que les gens les programmeraient pour qu'ils jouent des lignes de basse d'accompagnement pour jammer. Malheureusement, il y avait un certain nombre de problèmes : ils étaient un peu compliqués à programmer, ne sonnaient pas particulièrement bien pour remplacer une guitare basse et étaient assez chers à l'achat. Décidant d'arrêter les frais, Roland a cessé de les fabriquer après la vente de 10000 unités et, après quelques années passées sur les étagères des guitaristes, ils se sont rapidement retrouvés dans les vitrines des magasins d'occasion. Ces TB-303 solitaires et abandonnés attendaient d'être découverts par une nouvelle génération d'expérimentateurs qui ont commencé à les utiliser d'une manière que Roland n'avait pas imaginée pour créer de nouveaux sons fous. La Acid House était née.

Bien que mettre la main sur un TB-303 original ne soit pas si facile, vous serez heureux d'apprendre que vous pouvez transformer votre Raspberry Pi en TB-303 en utilisant la puissance de Sonic Pi. Lancez Sonic Pi, copiez ce code dans un tampon vide, et cliquez sur 'Run' :

```
use_synth :tb303
play :e1
```
    
De l'Acid Bass instantané ! Jouons un peu...

## Écrasez cette basse

Commençons par construire un arpégiateur interactif pour rendre les choses amusantes. Dans le dernier tutoriel, nous avons vu comment des mélodies peuvent n'être qu'un anneau de notes que nous parcourons les unes après les autres, en répétant quand nous arrivons à la fin. Créons une boucle interactive qui fait exactement cela :

```
use_synth :tb303
live_loop :squelch do
  n = (ring :e1, :e2, :e3).tick
  play n, release: 0.125, cutoff: 100, res: 0.8, wave: 0
  sleep 0.125
end
```

Jetez un œil à chaque ligne.

1. À la première ligne, choisissons `tb303` comme synthé par défaut avec la fonction `use_synth`.
2. À la deuxième ligne, créons une boucle interactive du nom de `:squelch` qui va juste boucler encore et encore.
3. La troisième ligne est l'endroit où nous créons notre riff : un anneau de notes (Mi dans les octaves 1, 2, et 3) que nous parcourons simplement avec `.tick'. Nous définissons `n' pour représenter la note courante dans le riff. Le signe égal signifie qu'il suffit d'assigner la valeur à droite au nom à gauche. Ce sera différent à chaque boucle. La première fois, `n' sera réglé sur `:e1`. La deuxième fois, ce sera `:e2`, suivi de `:e3`, puis retour à `:e1`, en bouclant à l'infini.
4. La ligne quatre est l'endroit où l'on déclenche notre synthé `:tb303`. On lui passe quelques options intéressantes : `release:`, `cutoff:`, `res:` et `wave:` que l'on décrira plus bas.
5. La ligne cinq est notre `sleep` : on demande à la boucle interactive de boucler toutes les `0.125`secondes ou 8 fois par seconde au BPM par défaut de 60.
6. La ligne six est la fin (NdT : `end`) de la boucle interactive. Le mot `end` indique précisément à Sonic Pi où se termine la boucle interactive.

Alors que vous êtes encore en train de vous familiariser avec ce qui se passe, tapez le code ci-dessous et cliquez sur le bouton 'Run'. Vous devriez entendre le `:tb303` entrer en action. C'est ici le cœur de l'action : commençons la programmation interactive.

Pendant que la boucle tourne, changez l'option `cutoff:` pour `110`. Puis cliquez à nouveau sur le bouton 'Run'. Vous devriez entendre le son devenir un peu plus dur et sec. Montez à `120` et cliquez sur 'Run'. Puis `130`. Écoutez comme les valeurs hautes de coupure (NdT : `cutoff`) rendent le son plus perçant et intense. Enfin, descendez à `80` quand vous sentirez qu'il vous faut un peu de repos. Puis répétez ça autant que vous voulez. Pas d'inquiétude, je serai toujours là...

Une autre option avec laquelle ça vaut le coup de jouer est `res:`. Elle contrôle le niveau de résonance du filtre. Une résonance haute est caractéristique des sons de basse acide. Nous avons pour le moment une `res:` de `0.8`. Essayez de la monter à `0.85`, puis `0.9`, et enfin `0.95`. Vous pourriez trouver qu'un filtre de coupure à `110` ou plus haut peut permettre d'entendre plus facilement les différences. Enfin défoulez-vous et montez jusque `0.999` pour avoir des sons déments. Avec une `res` aussi haute, vous entendez tellement le filtre de coupure résonner qu'il commencera à faire ses propres sons !

Enfin, pour avoir un grand impact sur le timbre, essayez de mettre l'option `wave:` à `1`. C'est le choix de l'oscillateur source. La valeur par défaut est `0` ce qui est une onde en dents de scie. `1` est une onde pulsée, et `2` est une onde en triangle.

Bien sûr, essayez différentes mélodies en changeant les notes dans l'anneau ou même en choisissant des notes de gammes ou d'accords. Amusez-vous bien avec votre premier synthé de basse acide.

## Déconstruisons le TB-303

Le design du TB-303 original était en fait assez simple. Comme vous pouvez le voir sur le diagramme suivant, il n'y a que quatre parties principales.

![Design du TB-303](../../../etc/doc/images/tutorial/articles/A.05-acid-bass/tb303-design.png)

Tout d'abord, il y a l'onde oscillatoire - les ingrédients de base du son. Dans ce cas, nous avons une onde carrée. Ensuite, il y a l'enveloppe d'amplitude de l'oscillateur qui contrôle l'amplitude de l'onde carrée au cours du temps. On peut y accéder dans Sonic Pi avec les options `attack:`, `decay:`, `sustain:` et `release:` ainsi qu'à leurs niveaux correspondant. Pour plus d'informations, lisez la Section 2.4 'Durée avec les enveloppes' dans le tutoriel intégré. Nous passons ensuite notre onde carrée enveloppée à travers un filtre passe-bas résonant. Cela coupe les hautes fréquences tout en ayant un bel effet de résonance. C'est ici que le plaisir commence. La valeur de coupure de ce filtre est également contrôlée par sa propre enveloppe ! Cela signifie que nous avons un contrôle incroyable sur le timbre du son en jouant avec ces deux enveloppes. Jetons un coup d’œil :

```
use_synth :tb303
with_fx :reverb, room: 1 do
  live_loop :space_scanner do
    play :e1, cutoff: 100, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
    sleep 8
  end
end
```
    
Pour chaque option d'enveloppe standard, il y a une option équivalente `cutoff_' dans le synthétiseur `:tb303'. Donc, pour changer le temps d'attaque de coupure, nous pouvons utiliser l'option `cutoff_attack:`. Copiez le code ci-dessus dans un tampon vide et cliquez sur 'Run'. Vous entendrez un bruit de gazouillis fou qui rentre et qui sort. Maintenant, commencez à jouer. Essayez de changer la durée de `cutoff_attack:` en `1' puis `0.5'. Maintenant, essayez `8`.

Remarquez que j'ai passé tout cela à travers un effet `:reverb` pour plus d'atmosphère - essayez d'autres effets pour voir ce qui fonctionne !

## Rassemblons tout

Enfin, voici un morceau que j'ai composé en utilisant les idées de ce tutoriel. Copiez-le dans un tampon vide, écoutez-le pendant un moment, puis commencez à coder interactivement vos propres modifications. Voyez quels sons fous vous pouvez en faire ! À la prochaine....

```
use_synth :tb303
use_debug false
 
with_fx :reverb, room: 0.8 do
  live_loop :space_scanner do
    with_fx :slicer, phase: 0.25, amp: 1.5 do
      co = (line 70, 130, steps: 8).tick
      play :e1, cutoff: co, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
      sleep 8
    end
  end
 
  live_loop :squelch do
    use_random_seed 3000
    16.times do
      n = (ring :e1, :e2, :e3).tick
      play n, release: 0.125, cutoff: rrand(70, 130), res: 0.9, wave: 1, amp: 0.8
      sleep 0.125
    end
  end
end
```
