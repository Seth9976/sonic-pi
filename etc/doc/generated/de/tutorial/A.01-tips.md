A.1 Tipps zu Sonic Pi

# Fünf Top-Tipps

## 1. Es gibt keine Fehler

Die wichtigste Lektion mit Sonic Pi ist, dass es wirklich keine Fehler gibt. Der beste Weg zu lernen, ist einfach es zu versuchen und zu versuchen und zu versuchen. Probiere viele verschiedene Dinge aus, kümmere dich nicht darum, ob dein Programm-Code gut oder schlecht klingt und fange an mit so vielen unterschiedlichen Synths, FXs und Optionen wie möglich zu experimentieren. Du wirst viele Dinge entdecken, die dich zum Lachen bringen, weil sie furchtbar klingen und auch richtige Edelsteine, die einfach toll klingen. Lass die Dinge, die dir nicht gefallen einfach beiseite und behalte, was dir gefällt. Je mehr 'Fehler' du dir zu machen erlaubst, desto schneller wirst du lernen und deinen eigenen Coding-Sound entdecken.


## 2. Verwende die Effekte

Sagen wir mal, Du beherrschst die Grundlagen, wie man in Sonic Pi Sounds mit `sample`und `play` macht. Was jetzt? Hast Du gewusst, dass Sonic Pi über 27 Studio-FX unterstützt, mit denen Du den Sound deines Codes verändern kannst? FX sind sowas wie raffinierte Bildeffekte in Bildbearbeitungsprogrammen; nur das sie nicht unscharf oder schwarz/weiß machen, sondern dem Sound Hall, Verzerrung oder Echo hinzufügen. Stell' Dir vor, wie Du das Kabel von Deiner Gitarre ins Effekt-Pedal steckst und dann in den Verstärker. Glücklicherweise braucht man keine Kabel, und Sonic Pi macht es sehr einfach, FX einzusetzen. Du musst nur auswählen, auf welchen Teil Deines Codes Du einen FX anwenden willst und diesen Teil mit dem FX-Code umschließen. Sehen wir uns ein Beispiel an:

```
sample :loop_garzul
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Wenn Du einen FX auf das `:loop_gazul`-Sample anwenden möchtest, steckst du es einfach in einen `with_fx`-Block, und zwar so:

```
with_fx :flanger do
  sample :loop_garzul
end
16.times do
  sample :bd_haus
  sleep 0.5
end
```

Wenn Du jetzt einen FX zur Bassdrum hinzufügen möchtest, dann packe diese auch in ein `with_fx` ein:

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

Denk' dran, Du kannst *jeden* Code mit einem `with_fx`umgeben und jeder Sound, der ausgegeben wird, geht durch diesen FX.


## 3. Parametrisiere Deine Synths

Um Deine codierten Klänge so richtig entdecken zu können, wirst Du sicher bald wissen wollen, wie Du Synths und FX steuern kannst. Vielleicht möchtest Du die Dauer eines Tons verändern, mehr Hall hinzufügen oder die Zeit zwischen zwei Echos verändern. Mit optionalen Parametern oder kurz Opts bietet Dir Sonic Pi viele Möglichkeiten, genau das zu tun. Schauen wir uns das mal kurz an. Kopiere diesen Code in einen Puffer und klicke auf Ausführen:

```
sample :guit_em9
```

Oh, was für ein wunderbarer Gitarren-Sound! Spielen wir ein bisschen damit. Wie wäre es damit, die Abspielgeschwindigkeit (rate) zu ändern?

```
sample :guit_em9, rate: 0.5
```

Was bedeutet der Schnipsel `rate: 0.5`, den ich hier am Ende hinzugefügt habe? Das ist ein Parameter. Alle Synths und FX in Sonic Pi unterstützen diesen und man kann viel damit anstellen. Versuche mal das hier:

```
with_fx :flanger, feedback: 0.6 do
  sample :guit_em9
end
```

Jetzt setze das feedback auf 1 und hör' Dir die verrückten Sounds an! Einzelheiten zu den vielen Opts, die Dir zur Verfügung stehen, findest Du in der Dokumentation.


## 4. Live Code

Die beste Art Sonic Pi schnell kennenzulernen, ist, live zu coden. Du fängst mit irgendeinem Codeschnipsel an und veränderst und verbesserst, während der Code abgespielt wird. Wenn Du zum Beispiel nicht weißt, was der Cutoff-Parameter mit einem Sample macht, probiere es einfach aus. Fangen wir mal an! Kopiere diesen Code in einen Puffer von Sonic Pi:

```
live_loop :experiment do
  sample :loop_amen, cutoff: 70
  sleep 1.75
end
```

Jetzt klicke auf ausführen und Du wirst einen leicht muffigen Drum-Break hören. Ändere den `cutoff:`-Wert auf `80` und klicke wieder ausführen. Hörst Du den Unterschied? Versuche es mit `90`, `100`, `110`...

Wenn Du die `live_loop`s einmal im Griff hast, willst du nichts mehr anders verwenden. Wann immer ich einen Live-Coding-Gig habe, brauche ich die `live_loop` wie ein Schlagzeuger seine Sticks. Um mehr über Live-Coding zu erfahren, sieh Dir den Abschnitt 9 im Tutorial an.

## 5. Mit dem Zufall spielen

Manchmal mogele ich, indem ich Sonic Pi für mich komponieren lasse. Mit Randomisierung lässt sich das großartig hinbekommen; das klingt vielleicht ein wenig kompliziert, ist es aber gar nicht. Sehen wir uns das an. Kopiere diesen Code in einen Puffer:

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

Wenn Du das laufen lässt, wirst Du eine regelmäßige Reihenfolge von zufälligen Tönen der Skala `:e2 :minor_pentatonic` gespielt vom `:dsaw`-Synth hören. "Halt mal, das ist doch keine Melodie", höre ich Dich schon sagen. Okay, dies ist der erste Teil des Zaubertricks. Jedes Mal, wenn die `live_loop` durchgelaufen ist, können wir Sonic Pi sagen, es soll die Zufallsfolge an einen bestimmten Punkt zurücksetzen. Das ist ein bisschen so, als würden wir wie Dr. Who in seiner Zeitmaschine TARDIS an einen bestimmten Ort und zu einer bestimmten Zeit zurückkehren. Versuche es mal und schreibe die Zeile `use_random_seed 1` in die `live_loop`:

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

Jedes Mal, wenn die `live_loop`sich wiederholt, wird der Zufalls-Generator zurückgesetzt. Das bedeutet, es werden jedes mal die selben 16 Noten ausgewählt. Hey presto, eine Melodie! Doch jetzt kommt der richtig spannende Teil: Ändere den Wert von `seed` von `1` in irgendeine andere Zahl. Sagen wir z.B. `4923`. Wow! Eine andere Melodie! Also nur durch das Ändern einer Zahl (dem so genannten `random seed`), kannst Du jegliche nur vorstellbare melodische Kombinationen erforschen! Na, wenn das nicht die Magie des Codes ist.
