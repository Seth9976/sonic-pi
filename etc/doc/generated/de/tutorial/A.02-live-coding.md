A.2 Live Coding

# Live-Coding

Die Laserstrahlen schnitten durch die Rauschwaden als der Subwoofer den Bass tief in die Körper der Menge pumpte. Die Atmosphäre war erfüllt von einem berauschenden Mix aus Synthesizern und Tanzen. Aber irgendetwas stimmte nicht in diesem Nachtklub. Über der DJ-Kabine leuchtete in bunten Farben futuristischer Text; bewegte sich, tanzte, blinkte. Dies war keine abgefahrene Lichtshow, sondern einfach eine Projektion von Sonic Pi, das auf einem Raspberry Pi lief. Der Insasse der DJ-Kabine drehte keine Platten, nein, er schrieb und bearbeitete Programmcode. Live. Das ist Live-Coding.

![Sam Aaron Live-Coding](../../../etc/doc/images/tutorial/articles/A.02-live-coding/sam-aaron-live-coding.png)

Es mag sich wie eine weit hergeholte Geschichte aus einem futuristischen Nachtklub anhört, aber Musik auf diese Weise zu programmieren, ist ein wachsender Trend, bekannt als Live-Coding (http://toplap.org). Eine aktuelle Richtung, die diese Art des Musikmachens eingeschlagen hat, ist der Algorave (http://algorave.com) - Veranstaltungen, auf denen Künstler wie ich Musik zum Tanzen programmieren. Du musst aber zum Live-Coden nicht in einem Nachtklub sein. Mit Sonic Pi v2.6+ kannst du überall loslegen, wohin du deinen Raspberry Pi und ein Paar Kopfhörer oder Lautsprecher mitnimmst. Wenn du das Ende dieses Artikels erreicht hast, wirst du bereits deine eigenen Beats programmieren und live bearbeitest. Wohin do von dort weiter gehst, wird einzig von deiner Vorstellungskraft beschränkt.

## Live-Loop

Der Schlüssel zum 'live-coding' mit Sonic Pi ist das Beherrschen des 'live_loops'. Schauen wir uns einen an:

```
live_loop :beats do
  sample :bd_haus
  sleep 0.5
end
```

Ein Live-Loop hat 4 Hauptbestandteile. Der erste ist sein Name. Unser `live_loop` oben heißt `live_loop`. Du kannst frei entscheiden, wie Du Deinen `live_loop` nennen möchtest. Sei kreativ. Ich benutze oft Namen, die dem Publikum etwas über die Musik mitteilen, die ich mache. Der zweite Bestandteil ist das Wort `do`, welches anzeigt, wo der `live_loop` beginnt. Der dritte ist das Wort `end`, das markiert, wo der `live_loop` endet. Schließlich gibt es noch den Block innerhalb des `live_loop`, der beschreibt, was die Schleife wiederholen soll – das ist der Teil zwischen `do` und `end`. In unsrem Fall spielen wir ein Bass-Drum-Sample und warten einen halben Takt. Dies führt zu einem schönen regelmäßigen Bass Beat. Auf gehts, kopiere den `live_loop` in einem leeren Sonic Pi-Puffer und drücke auf Ausführen. Boom, Boom, Boom!.

## Zur Laufzeit neu definieren

OK, aber was ist nun das besondere an einem `live_loop`? Bisher scheint er nur eine überbewertete "Schleife" zu sein! Nun, das Schöne am `live_loop` ist, dass du ihn im laufenden Programm neu definieren kannst. Das bedeutet, du kannst ändern was er machen soll, während er läuft. Das ist das Geheimnis hinter Live-Coding mit Sonic Pi. Lass uns das ausprobieren:

```
live_loop :choral_drone do
  sample :ambi_choir, rate: 0.4
  sleep 1
end
```

Klicke auf die `Ausführen`-Schaltfläche oder drücke `Alt-R`. Du hörst jetzt einen wunderschönen Chor-Klang. Nun, während dieser noch läuft, ändere deie Rate von `0.4` auf `0.38`. Klicke erneut `Ausführen`. Wow! Hast du gehört, wie der Chor die Note gewechselt hat? Setze sie wieder auf `0.4`, zurück auf den alten Wert. Nun setze ihn runter auf `0.2`, runter bis `0.19` und dann wieder hoch auf `0.4`. Sieh wie du durch das Ändern nur eines Parameters, im laufenden Programm, die volle Kontrolle über die Musik erlangen kannst? Spiele ein bisschen mit den Werten für `rate` - wähle deine eigenen Werte. Probiere negative Zahlen, wirklich kleine Zahlen und große Zahlen. Viel Spaß!

## Schlafen ist wichtig

Eine der wichtigsten Lektionen über 'live_loop's ist, dass sie Pausen brauchen. Betrachte einmal folgenden 'live_loop':

```
live_loop :infinite_impossibilities do
  sample :ambi_choir
end
```

Wenn du versuchst, diese Code auszuführen, wirst du bemerken, dass Sonic Pi sich beschwert, dass der `live_loop` nicht geschlafen hat. Das ist ein Sicherheitssystem! Nimm dir etwas Zeit und denk darüber nach, was dieser Code vom Computer verlangt. Genau, der Computer wird gefragt eine unendliche Anzahl an Samples zum Nullzeitpunkt zu spielen. Ohne das Sicherheitssystem wird der arme Computer das probieren und währenddessen abstürzen. Also immer daran denken: Deine `live_loop`s müssen ein `sleep` beinhalten.


## Töne kombinieren

Musik ist voll von Dingen, die zur selben Zeit geschehen. Das Schlagzeug spielt zur selben Zeit wie der Bass, Gesang und die Gitarre… In der Informatik nennen wir das Nebenläufigkeit (concurrency). Sonic Pi bietet uns eine einfache Möglichkeit verschiedenste Dinge zur selben Zeit abspielen zu lassen. Benutze einfach mehr als einen `live_loop`!

```
live_loop :beats do
  sample :bd_tek
  with_fx :echo, phase: 0.125, mix: 0.4 do
    sample  :drum_cymbal_soft, sustain: 0, release: 0.1
    sleep 0.5
  end
end
live_loop :bass do
  use_synth :tb303
  synth :tb303, note: :e1, release: 4, cutoff: 120, cutoff_attack: 1
  sleep 4
end
```

Hier haben wir zwei `live_loop`s. Der eine wiederholt schnell Beats, während der andere durch langsames Wiederholen einen verrückten Bass Sound kreiert.

Eines der interessanten Dinge bei der Verwendung von `live_loop`in mehreren Instanzen ist, dass sie alle jeweils ihre eigene Zeit verwalten. Das bedeutet, dass es wirklich einfach ist, interessante polyrhythmische Strukturen zu erzeugen und sogar mit Phasenverschiebungen á la Steve Reich zu spielen. Schau dir das an:

```
# Steve Reich's Piano Phase
notes = (ring :E4, :Fs4, :B4, :Cs5, :D5, :Fs4, :E4, :Cs5, :B4, :Fs4, :D5, :Cs5)
live_loop :slow do
  play notes.tick, release: 0.1
  sleep 0.3
end
live_loop :faster do
  play notes.tick, release: 0.1
  sleep 0.295
end
```

## Alles zusammenführen

Jedes dieser Tutorials werden wir mit einem abschließenden Code-Beispiel beenden, das sich aller zuvor behandelter Ideen bedient. Schau Dir diesen Code genau an und überlege Dir, was er tut. Kopiere ihn anschließend in einen neuen Sonic Pi-Puffer, drücke auf Start und hör Dir an, wie er klingt. Ändere zum Schluss etwas am Code, indem Du Parameter veränderst oder Dinge auskommentierst. Vielleicht kannst Du das als Ausgangspunkt für eine eigene Performance nutzen. Das Wichtigste ist auf jeden Fall der Spaß dabei. Bis zum nächsten Mal …

```
with_fx :reverb, room: 1 do
  live_loop :time do
    synth :prophet, release: 8, note: :e1, cutoff: 90, amp: 3
    sleep 8
  end
end
live_loop :machine do
  sample :loop_garzul, rate: 0.5, finish: 0.25
  sample :loop_industrial, beat_stretch: 4, amp: 1
  sleep 4
end
live_loop :kik do
  sample :bd_haus, amp: 2
  sleep 0.5
end
with_fx :echo do
  live_loop :vortex do
    # use_random_seed 800
    notes = (scale :e3, :minor_pentatonic, num_octaves: 3)
    16.times do
      play notes.choose, release: 0.1, amp: 1.5
      sleep 0.125
    end
  end
end
```
