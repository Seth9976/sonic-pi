A.4 Synth Riffs

# Synth Riffs

Ob polterndere Oszillatoren oder die verstimmten Klänge von Sägezahnschwingungen, die sich durch ein Stück ziehen, der Lead Synth - der melodische Hauptpart eines Stücks - spielt in jeder elektronischen Komposition eine wichtige Rolle. Im der letzten Einheit dieses Tutorials haben wir gelernt, wie man Klänge erzeugt. Jetzt werden wir uns damit beschäftigen, wie wir die drei Hauptkomponenten eines Synth Riffs - Klangfarbe, Melodie und Rhythmus - coden können.

OK, fahre deinen Raspberry Pi hoch, öffne Sonic Pi v2.6+ und auf gehts, lass und Musik machen!


## Die Welt der Klangfarben

Ein essentieller Teil jeder Synth-Figur ist das Verändern und Spielen mit der Färbung der Klänge. Wir können die Klangfarbe in Sonic Pi Sonic Pi auf zwei Arten steuern - indem wir für dramatische Änderungen verschiedene Synths verwenden, und für dezente Modifizierungen die verschiedenen Synth-Optionen einsetzen. Wir können dafür auch Effekte einsetzen, aber das ist ein Tutorial für sich …

Lass uns einen einfachen Live-Loop schreiben, in dem wir laufend den Synth ändern:

```
live_loop :timbre do
  use_synth (ring :tb303, :blade, :prophet, :saw, :beep, :tri).tick
  play :e2, attack: 0, release: 0.5, cutoff: 100
  sleep 0.5
end
```

Schau dir den Code genauer an. Mit dem `tick` Befehl gehen wir Eintrag für Eintrag durch einen Ring von Synth Namen (wobei wir die Liste immer wieder wiederholen). Anschließend übergeben wir diesen Synth an die `use_synth` Funktion, die den aktuellen Synth unseres Live-Loops ändert. Außerdem spielen wir die Note `:e2` (e der zweiten Oktave) mit einer Abklingzeit von 0.5 Takten (0.5 Sekunden bei unser Standard-BPM von 60) und einem `cutoff:` Wert von 100.

Hörst du, wie die unterschiedlichen Synths vollkommen verschiedene Klänge erzeugen, obwohl sie alle die selbe Note spielen? Lass uns damit experimentieren. Erhöhe oder verkleinere den Wert der Abklingzeit. Ändere zum Beispiel die Werte der Optionen `attack:` und `release:`, um zu sehen, wie sich unterschiedliche Fade-In und Fade-Out Zeiten auf den Klang auswirken. Zuletzt kannst du den Wert der `cutoff:` Option ändern, um zu beobachten, wie unterschiedliche Cut-Off-Wert die Klangfarbe beeinflussen (Werte zwischen 60 und 130 sind gut). Probiert mal aus, wie viele verschiedene Klänge du erzeugen kannst, indem du an diesen wenigen Parametern rumschraubst. Wenn dir das gelingt, kannst du im dir im Help System den Eintrag zu Synth anschauen. Hier findest du eine Auflistung aller Synths und der Optionen, die sie bereitstellen. Ein Reich an Möglichkeiten liegt dir zu Füßen.

## Klangfarbe

Klangfarbe (engl. timbre) ist nur ein ausgefallenes Wort für den Klang eines Geräusches. Wenn man die selbe Note auf verschiedenen Instrumenten, wie zum Beispiel einer Geige, einer Gitarre oder einem Klavier spielt, so bleibt die Tonhöhe (wie hoch oder niedrig ein Ton ist) immer die selbe. Die Tonqualität hingegen unterscheidet sich. Dieser Unterschied, der einem erlaubt festzustellen, ob es sich um ein Klavier oder eine Gitarre handelt, ist die Klangfarbe.


## Melodische Komposition

Ein anderer wichtiger Aspekt in der Zusammenstellung unseres Leas Synths ist die Wahl der Noten, die gespielt werden sollen. Wenn du bereits eine Idee dafür hast, kannst du einfach einen Ring erstellen, der über die gewünschte Notenfolge iteriert:

```
live_loop :riff do
  use_synth :prophet
  riff = (ring :e3, :e3, :r, :g3, :r, :r, :r, :a3)
  play riff.tick, release: 0.5, cutoff: 80
  sleep 0.25
end
```
    
Hier haben wir unsere Melodie mit Hilfe eines Rings definiert, der sich aus Noten wie `:e3` und Pausen - dargestellt durch `:r`- zusammensetzt. Wir nutzen `.tick` um über die Notenfolge zu iterieren und so einen sich wiederholenden Riff zu erzeugen.

## Automatische Melodie

Es ist nicht leicht einen gut klingenden Riff aus dem Nichts zu zaubern. Statt dessen ist es oft hilfreich sich von Sonic Pi eine Auswahl von zufälligen Riffs ausgeben zu lassen und einen von diesen auszuwählen. Um das zu tun, verbinden wir drei bekannte Konzepte miteinander: Ringe, Randomisierung und Zufallszahlen. Schauen wir uns ein Beispiel an:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 3
  notes = (scale :e3, :minor_pentatonic).shuffle
  play notes.tick, release: 0.25, cutoff: 80
  sleep 0.25
end
```

Es gibt ein paar Dinge, die wir uns der Reihe nach ansehen wollen. Zunächst legen wir fest, dass wir den Zufallswert 3 verwenden. Was bedeutet das? Nun, das Nützliche daran ist, dass wir, wenn wir den Seed setzen, vorhersagen können, was der nächste Zufallswert sein wird - es ist derselbe wie beim letzten Mal, als wir den Seed auf 3 gesetzt haben! Eine weitere nützliche Information ist, dass das Mischen eines Notenrings auf dieselbe Weise funktioniert. Im obigen Beispiel fragen wir im Wesentlichen nach dem "dritten Shuffle" in der Standardliste der Shuffles - was auch jedes Mal dasselbe ist, da wir den Zufallswert immer auf denselben Wert setzen, kurz bevor wir den Shuffle starten. Schließlich gehen wir einfach durch unsere gemischten Noten, um das Riff zu spielen.

Jetzt fängt der Spaß erst so richtig an. Wenn wir den Startwert(Seed) für den Zufallsgenerator auf eine andere Zahl, z. B. 3000, ändern, erhalten wir eine völlig andere Mischung der Noten. So ist es jetzt extrem einfach, neue Melodien zu erforschen. Wähle einfach die Liste der Noten aus, die du mischen möchten (Tonleitern sind ein guter Ausgangspunkt), und wählen Sie dann den Startwert(Seed), mit dem du mischen möchten. Wenn uns die Melodie nicht gefällt, ändern wir einfach einen dieser beiden Punkte und versuchen es erneut. Wiederhole den Vorgang, bis dir gefällt was du hörst!


## Pseudo-Randomisierung

Die Zufallsgenerierung von Sonic Pi ist nicht wirklich zufällig, sondern wird als Pseudozufall bezeichnet. Stell Dir vor, Du würdest 100 Mal würfeln und das Ergebnis jedes Wurfs auf ein Blatt Papier schreiben. Sonic Pi verfügt über das Äquivalent dieser Ergebnisliste, die verwendet wird, wenn man nach einem Zufallswert fragt. Anstatt zu würfeln, wird einfach der nächste Wert auf der Liste ausgewählt. Wenn Du einen Startwert(Seed) setzt, dann springst Du einfach zu einem bestimmten Punkt in dieser Liste.
 
## Finde deinen Rythmus

Ein weiteres wichtiges Merkmal unseres Riffs ist der Rhythmus, d.h. wann wir eine Note spielen und wann nicht. Wir haben bereits gesehen, dass wir `:r` nutzen können, um Pausen in einen Ring einzufügen. Eine weiter Möglichkeit auf den Rhythmus Einfluss zu nehmen sind sog. Spreads, über die wir in einem kommenden Tutorial mehr lernen werden. Heute nutzen wir Randomisierung, um unseren Rhythmus zu finden. Anstatt immer jede Note in einem Ring zu spielen, können wir über eine Bedingung festlegen, mit welcher Wahrscheinlichkeit sie abgespielt werden. Lass uns einen Blick darauf werfen:

```
live_loop :random_riff do
  use_synth :dsaw
  use_random_seed 30
  notes = (scale :e3, :minor_pentatonic).shuffle
  16.times do
    play notes.tick, release: 0.2, cutoff: 90 if one_in(2)
    sleep 0.125
  end
end
```

Eine nützliche Funktion in diesem Zusammenhang ist die Funktion `one_in`, die `true` bzw. `false` mit einer bestimmten Wahrscheinlichkeit zurückgibt. Hier verwenden wir den Wert 2, d.h. `one_in` gibt durchschnittlich ein Mal alle zwei Aufrufe `true` zurück. Mit anderen Worten, `true` wird in 50% der Fälle zurückgegeben. Höhere Werte bewirken, dass statt `true` häufiger `false` zurückgegeben wird. Das führt zu mehr Lücken in unserem Riff.

Beobachte, dass wir mit dem Befehl `16.times` Wiederholung eingebaut haben. Das haben wir gemacht, damit sich unser Zufallsgenerator (der Rückgabewert der `one_in` Funktion) nur alle 16 Noten zurücksetzt und unser Rhythmus sich so alle 16 Schlägen wiederholt. Wir nehmen damit keinen Einfluss auf das durcheinander Mischen, weil letzteres direkt nach dem Initiieren des Zufallsgenerators passiert. Wir können über die Größe der Wiederholungen die Länge unseres Riffs verändern. Versuche mal die Zahl 16 auf 8 oder sogar 4 oder 3 zu ändern und schaue dir an, wie sich das auf den Rhythmus des Riffs auswirkt.

## Alles zusammenführen

OK, lass uns zum Schluss alles, das wir gelernt haben, in einem abschließenden Beispiel nutzen. Bis zum nächsten Mal!

```
live_loop :random_riff do
  #  uncomment to bring in:
  #  synth :blade, note: :e4, release: 4, cutoff: 100, amp: 1.5
  use_synth :dsaw
  use_random_seed 43
  notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle.take(8)
  8.times do
    play notes.tick, release: rand(0.5), cutoff: rrand(60, 130) if one_in(2)
    sleep 0.125
  end
end
 
live_loop :drums do
  use_random_seed 500
  16.times do
    sample :bd_haus, rate: 2, cutoff: 110 if rand < 0.35
    sleep 0.125
  end
end
 
live_loop :bd do
  sample :bd_haus, cutoff: 100, amp: 3
  sleep 0.5
end
```
