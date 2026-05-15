4 Zufälligkeit

# Zufälligkeit

Eine tolle Möglichkeit deine Musik interessant zu gestalten ist die Nutzung von Zufallszahlen. Sonic Pi bietet einige tolle Funktionen, um deiner Musik Zufälligkeiten hinzuzufügen. Aber bevor wir damit anfangen, musst du noch eine schockierende Wahrheit erfahren: In Sonic Pi bedeutet *zufällig nicht wirklich zufällig*. Was zum Teufel soll das heißen? Nun, das werden wir sehen.

## Wiederholbarkeit

Eine wirklich nützliche Zufallsfunktion ist `rrand`. Sie liefert dir einen zufälligen Wert zwischen zwei Zahlen - einem *Minimal-* und einem *Maximalwert* - (`rrand` ist ein Kürzel für *ranged random*, also eine Zufallszahl aus einem bestimmten Zahlenbereich). Versuchen wir mal eine zufällig ausgewählte Note zu spielen:

```
play rrand(50, 95)
```

Oh, es hat eine zufällige Note gespielt. Es hat Note `83.7527`gespielt. Eine nette zufällige Note zwischen 50 und 100. Aber halt, habe ich gerade diese angeblich zufällige Note exakt bestimmt? Da ist doch etwas nicht ganz sauber. Lasse den Programm-Code noch einmal laufen. Wie bitte? Es hat wieder `83.7527`gewählt? Das kann kein Zufall sein!

Die Antwort ist, es ist nicht wirklich zufällig, es ist pseudo-zufällig. Sonic Pi liefert dir zufallsähnliche Zahlen, die sich wiederholen. Das ist sehr nützlich, um sicherzustellen, dass die Musik die du auf deinem Rechner erzeugst, auf jedem anderen Rechner identisch klingt - auch dann, wenn du Zufälligkeiten in deiner Komposition verwendest.

Klar, wenn es, in einem bestimmten Musikstück, jedes Mal 'zufällig' die `83.7527` wählen würde, dann wäre das nicht besonders interessant. Aber das tut es auch nicht. Probiere folgendes:

```
loop do
  play rrand(50, 95)
  sleep 0.5
end 
```

Ja! Jetzt klingt es zufällig. Innerhalb eines gegebenen *Programmlaufes* liefern weitere Aufrufe von Zufallsfunktionen auch zufällige Werte. Trotzdem wird der nächste Programmlauf genau die selbe Abfolge von Zufallswerten liefern und entsprechend auch genau gleich klingen. Es ist als ob Sonic Pi Code immer an denselben Zeitpunkt zurückspringt, wenn der Ausführen-Schalter geklickt wird. Das ist der Murmeltier-Tag der musikalischen Synthese!

## Gesiterglocken

Ein schönes Beispiel von Zufälligkeit in Aktion ist das Geisterglocken-Beispiel, in dem das `:perc_bell`-Sample mit einer zufälligen Samplerate und Pausenzeit zwischen den Glockenklängen abgespielt wird:

```
loop do
  sample :perc_bell, rate: (rrand 0.125, 1.5)
  sleep rrand(0.2, 2)
end
```

## Zufällige Begrenzung

Ein anderes unterhaltsames Beispiel für Zufälligkeit ist, einen Synth-Klang zufällig in der Tonhöhe zu begrenzen. Ein toller Synth um das auszuprobieren ist der `:tb303`-Emulator:

```
use_synth :tb303
loop do
  play 50, release: 0.1, cutoff: rrand(60, 120)
  sleep 0.125
end
```

## Zufallsstartpunkte

Was, wenn dir eine bestimmte Zufallsabfolge, die Sonic Pi liefert, dir nicht gefällt? Nun, mit `use_random_seed` lässt sich sehr gut ein anderer Startpunkt für diese Zufallsabfolge wählen. Der Standard-Startpunkt ist die 0, wähle also einen anderen Startpunkt, und du machst eine andere Zufallserfahrung!

Sieh dir folgenden Code an:

```
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Jedes Mal, wenn du diesen Programm-Code laufen lässt, hörst du dieselbe Folge von 5 Noten. Um eine andere Folge zu bekommen, setze einen anderen Startpunkt:

```
use_random_seed 40
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

Nun produziert Sonic Pi eine andere Folge aus 5 Tönen. Indem du den Startpunkt änderst und dir die Ergebnisse anhörst, kannst du eine Folge finden, die dir gefällt - und wenn du den Code an andere weitergibst, werden sie genau das hören, was auch du gehört hast.

Schauen wir uns noch einige andere nützliche Zufallsfunktionen an.


## Auswählen

Eine Sache, die sehr häufig gemacht wird, ist aus einer Liste bekannter Elemente eines zufällig auszuwählen. Zum Beispiel möchte ich vielleicht einen Ton aus der folgenden Liste spielen: 60, 65 oder 72. Das kann ich mit `choose` erreichen, das mich ein Element aus einer Liste wählen lässt. Zuerst musst ich meine Zahlen in eine Liste packen, indem ich sie, durch Kommas getrennt, in eckige Klammern setze. Danach muss ich sie einfach nur an `choose` übergeben:

```
choose([60, 65, 72])
```

Hören wir uns das mal an:

```
loop do
  play choose([60, 65, 72])
  sleep 1
end
```

## rrand

Wir haben `rrand` schon kennengelernt, aber sehen wir uns das noch einmal genauer an. Es liefert eine zufällige Zahl zwischen zwei Werten - ausschließlich dieser Werte selbst. Das bedeutet, dass weder der minimale noch der maximale Wert jemals ausgegeben werden, immer nur was zwischen den beiden liegt. Diese Zahl wird immer eine Gleitkommazahl (floating point number) sein - also keine ganze Zahl, sondern eine Bruchzahl (Erinnere dich: Computer verwenden für Rechenoperationen stets die englische Schreibweise - daher Punkt, nicht Komma!). Hier Beispiele für Gleitkommazahlen beim Aufruf von `rrand(20, 110)`:

* 87.5054931640625
* 86.05255126953125
* 61.77825927734375

## rrand_i

Gelegentlich wirst du jedoch eine ganze Zahl wollen, keine Gleitkommazahl. Hier kommt `rrand_i` (*i* für englisch integer, ganze Zahl) zur Hilfe. Es funktioniert ähnlich wie `rrand`, kann jedoch auch den minimalen oder maximalen Wert als mögliche Zufallszahl liefern. (was bedeutet, dass es einschließlich der begrenzenden Werte funktioniert und nicht ausschließlich). Beispiele wären die Werte, die von `rand_i(20,110)` ausgeben werden:

* 88
* 86
* 62

## rand

Dies wird eine zufällige Gleitkommazahl zwischen 0 (inklusiv) und einem von dir spezifizierten Maximalwert (exklusiv) zurückgeben. Standardmäßig wird ein Wert zwischen 0 und 1 ausgegeben. Daher ist es nützlich für eine Auswahl zufälliger Werte für `amp:`:

```
loop do
  play 60, amp: rand
  sleep 0.25
end
```

## rand_i

Ähnlich wie im Verhältnis von `rrand_i` zu `rrand`, wird `rand_i` eine zufällige ganze Zahl zwischen 0 und dem angegebenen Maximalwert zurückgeben.

## dice

Manchmal wirst du so tun wollen, als würdest du würfeln (dice) - dies ist ein Sonderfall von `rrand_i`, bei dem der kleinste Wert immer die 1 ist. Ein Aufruf von `dice` verlangt von dir die Anzahl von Seiten zu bestimmen, die der Würfel haben soll. Ein normaler Würfel hat 6 Seiten, `dice(6)` wird dem entsprechend einen der Werte 1, 2, 3, 4, 5 oder 6 zurückgeben. Wie auch immer, in einem Rollenspiel würdest auch Nutzen in einem 4-seitigen Würfel sehen, oder einem 12-seitigen, oder einem 20-seitigen, vielleicht sogar in einem 120-seitigem!

## one_in

Schließlich könnte es noch so sein, dass du so tun willst, als ob du beim Würfeln mit einem Standardwürfel eine 6 hast - also den höchsten Wert. `one_in` gibt dafür mit einer Wahrscheinlichkeit von 1 im Verhältnis zur Anzahl Würfelseiten den Wert wahr (true) zurück. Daher wird `one_in(6)` mit einer Wahrscheinlichkeit von 1 zu 6 wahr, ansonsten falsch (false). Wahr- und Falsch-Werte sind sehr nützlich bei `if`-Anweisungen, welche wir in einem späteren Abschnitt dieses Tutorials behandeln werden.

Jetzt los, bringe deinen Code mit ein wenig Zufälligkeit durcheinander!
