10 État du temps

# État du temps

Il est souvent utile d'avoir de l'information qui est *partagée à travers plusieurs fils d'exécution ou boucles en direct*. Par exemple, vous pourriez vouloir partager une notion de la clef courante, de BPM, ou des concepts encore plus abstraits comme la 'complexité' courante (que vous pourriez potentiellement interpréter de manière différente à travers divers fils). Nous ne voulons pas non plus perdre aucune nos garanties existantes de déterminisme en faisant cela. En d'autres mots, nous aimerions être toujours capable de partager du code avec d'autres, et savoir exactement ce qu'ils vont entendre en l'exécutant. À la fin de la Section 5.6 de ce tutoriel, nous avons discuté brièvement de pourquoi nous *ne devrions pas utiliser des variables pour partager l'information à travers les fils d'exécution* à cause d'une perte de déterminisme (ce qui est dû à la concurrence).

La solution de Sonic Pi au problème de travailler facilement avec les variables globales d'une manière déterministe est un système novateur appelé l'État du Temps. Cela peut sembler complexe et difficile (en fait, au Royaume-Uni, la programmation avec plusieurs fils d'exécution et de la mémoire partagée est habituellement un sujet de niveau universitaire). Toutefois, comme vous allez le voir, tout comme jouer votre première note, *Sonic Pi rend incroyablement simple le partage d'état entre les fils d'exécution* tout en conservant vos programmes *avec des fils d'exécution sûrs et déterministes.*.

Rencontrez `get` et `set`...
