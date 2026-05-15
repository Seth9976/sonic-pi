A.3 Kodierte Beats

# Kodierte Beats

Eine der spannendsten und einflussreichsten technischen Entwicklungen der modernen Musik ist die Erfindung von Samplern. Sampler sind Musikinstrumente, die es einem erlauben Töne aufzunehmen, sie zu verändern und auf verschiedene Arten und Weisen wiederzugeben. Zum Beispiel ist es mit einem Sampler möglich ein Schlagzeug Solo (oder Break) von einer alten Schalplatte einzuspielen und es anschließend als Grundlage für einen neuen Beat zu verwenden, indem man es mit halber Geschwindigkeit wiedergibt. So ist früher Hip-Hop entstanden und heute gibt es kaum noch Elektronische Musik, die ohne irgendeine Art von Samples auskommt. Samples bieten dir eine großartige Möglichkeit auf einfache Art und Weise neue und interessante Elemente in Deine live gecodete Performance einfließen zu lassen.

Aber wo kriegen wir einen Sampler her? Wir haben bereits einen - es ist unser Raspberry Pi! Die mitgelieferte Live-Coding App Sonic Pi stellt uns einen mächtigen Sampler bereit. Lasst ihn uns ausprobieren!

## Der Amen Break

Eines der klassischen Schlagzeug-Break-Samples mit dem größten Wiedererkennungswert ist der Amen-Break. Er wurde erstmals 1969 von den Winstons in ihrem Song "Amen Brother" als Teil eines Drum-Break aufgenommen. Doch erst seine Wiederentdeckung durch frühe Hip-Hop-Musiker in den 80ern und sein Einsatz in Samplern führte zu einer Verwendung des Samples in einer großen Bandbreite von Musikstilen, wie Drum and Bass, Breakbeat, Hardcore Techno und Breakcore.

Ich bin mir sicher, Du freust Dich zu hören, dass das Sample direkt in Sonic Pi eingebaut ist. Bereite einfach einen Puffer vor und kopiere den folgenden Code hinein:

```
sample :loop_amen
```

Drücke auf *Start* und boom! Du hörst dir gerade eines der einflussreichsten Drum Breaks in der Geschichte der Tanzmusik an. Dieses Sample ist allerdings nicht damit berühmt geworden, einmal abgespielt zu werden. Vielmehr ist es wie gemacht dafür wiederholt zu werden.


## Beat Stretching

Lass uns den Amen Break in Schleife schalten, indem wir unseren alten Bekannten, den`live_loop` aus dem Tutorial vom letzten Monat, einsetzen:

```
live_loop :amen_break do
  sample :loop_amen
  sleep 2
end
```

OK, es wiederholt sich. Allerdings kommt es am Ende eines Durchlaufs zu einer lästige Pause. Diese entsteht durch unserer Anweisung `2` Takte zu pausieren. Das `:loop_amen` Sample dauert bei einem voreingestellten BPM Wert von 60 nur `1.753` Takte. Das bedeutet kommt es am Sample Ende zu einer Pause von `2 - 1.753 = 0.247` Takten, was kurz, aber durchaus wahrnehmbar ist.

Um dieses Problem zu beheben, können wir die `beat_stretch:` Option verwenden. Sie sagt Sonic Pi, dass das Sample auf die angegebene Anzahl von Takten ausgedehnt (bzw. gestaucht) werden soll.

Sonic Pis Funktionen `sample` und `synth` geben uns über optionale Parameter wie `amp:`, `cutoff:` und `release:` viele zusätzliche Steuerungsmöglichkeiten. Da die Bezeichnung optionale Parameter allerdings recht lang ist, werden wir sie ab jetzt einfach *opts* nennen.

```
live_loop :amen_break do
  sample :loop_amen, beat_stretch: 2
  sleep 2
end  
```

Jetzt schwingen wir das Tanzbein! Vielleicht wollen wir es noch etwas schneller, oder doch einen Ton gemächlicher - je nach Stimmung.

## Mit der Zeit spielen

OK, wie sieht es aus, wenn wir den Stiel unserer Musik zu Hip Hop oder Breakcore ändern wollen? Eine einfache Möglichkeit das zu tun ist mit der Zeit zu spielen - oder in anderen Worten am Tempo herumbasteln. In Sonic Pi ist das super leicht - füge einfach `use_bpm` in deinen Live-Loop ein:

```
live_loop :amen_break do
  use_bpm 30
  sample :loop_amen, beat_stretch: 2
  sleep 2
end 
```

Während du gerade zu diesen langsamen Beats rappst, beobachte, dass obwohl wir immer noch eine Pause von 2 machen und unsere BPM bei 30 liegen nichts verzögert klingt. Die `beat_stretch` Option berücksichtigt den aktuellen BPM Wert und bringt alles in Einklang.

Jetzt kommen wir zum spannenden Teil. Während der Loop läuft, verändere den Wert `30` in der `use_bpm 30` Zeile zu `50`. Wuhuu, auf einmal ist alles *ohne aus dem Einklang zu geraten* schneller geworden! Versuch das Tempo noch etwas mehr zu erhöhen - 80, 120 und um es wirklich verrückt klingen zu lassen, trage 200 ein!


## Filtern

Nun können wir Samples in unseren Live-Loop integrieren. Schauen wir uns einige der interessanten Optionen des `sample` Synths an. Zunächst `cutoff:`, das den Cut-Off-Filter des Samplers steuert. Standardmäßig ist dieser ausgeschaltet. Aber du kannst ihn ganz einfach einschalten:

```
live_loop :amen_break do
  use_bpm 50
  sample :loop_amen, beat_stretch: 2, cutoff: 70
  sleep 2
end  
```

Nun, ändere den Wert der `cutoff:` Option. Erhöhe ihn zum Beispiel auf 100, drücke auf *Start* und warte bis der Loop einmal durchgelaufen ist, um die Änderung zu hören. Du kannst beobachten, dass niedrige Werte wie 50 den Klang voll und basslastig machen, hohe Werte wie 100 und 200 aber voll und kratzend klingen. Das liegt daran, dass die `cutoff:` Option die Höhen wegschneidet - genau so wie ein Rasenmäher die Enden von Grashalmen abschneidet. Die `cutoff:` fungiert als eine Längeneinstellung. Sie legt fest, wie viel Grass nach dem Mähen übrig bleibt.


## Slicing

Ein anderes tolles Tool, das wir ausprobieren können, ist der FX Slicer. Er stückelt unseren Sound in Einzelsequenzen. Verpacke die `sample` Zeile dafür einfach mit dem folgenden FX Code:

```
live_loop :amen_break do
  use_bpm 50
  with_fx :slicer, phase: 0.25, wave: 0, mix: 1 do
    sample :loop_amen, beat_stretch: 2, cutoff: 100
  end
  sleep 2
end
```

Beobachte, wie der Klang dadurch etwas mehr auf und ab federt. (Du kannst dir den ursprünglichen Klang ohne FX anhören, indem du die Option `mix:` auf `0` setzt.) Als nächstes, versuche dich an der `phase:` Option. Das ist die Rate (in Beats) in der gestückelt wird. Ein kleiner Wert wie `0.125` stückelt häufig, hohe Werte wie `0.5` stückeln hingegen langsamer. Beobachte, dass eine stufenweises Halbieren oder Vierteln der `phase:` tendenziell immer gut klingt. Setzte zuletzt die `wave:` Option auf 0, 1, oder 2 und höre dir an, wie die Änderung klingt. Die Werte stehen für unterschiedliche Schwingungsformen. 0 repräsentiert eine Sägezahnschwindung (hard in, fade out), 1 eine Rechteckschwingung (hard in, hard out) und 2 eine Dreieckschwingung (hard in, hard out).


## Alles zusammenführen

Lass uns für unser letztes Beispiel einem Blick auf die frühe Drum and Bass Szene in Bristol werfen. Mach dir keinen Sorgen, wenn du nicht genau verstehst, wie das Beispiel funktioniert. Füge den Code einfach in Sonic Pi ein, klicke auf Ausführen und versuche dich am Live-Coden, indem du die Werte der verschiedenen Optionen veränderst. Vergesse nicht deine Kreationen mit anderen zu teilen! Bis zum nächsten Mal…

```
use_bpm 100
live_loop :amen_break do
  p = [0.125, 0.25, 0.5].choose
  with_fx :slicer, phase: p, wave: 0, mix: rrand(0.7, 1) do
    r = [1, 1, 1, -1].choose
    sample :loop_amen, beat_stretch: 2, rate: r, amp: 2
  end
  sleep 2
end
live_loop :bass_drum do
  sample :bd_haus, cutoff: 70, amp: 1.5
  sleep 0.5
end
live_loop :landing do
  bass_line = (knit :e1, 3, [:c1, :c2].choose, 1)
  with_fx :slicer, phase: [0.25, 0.5].choose, invert_wave: 1, wave: 0 do
    s = synth :square, note: bass_line.tick, sustain: 4, cutoff: 60
    control s, cutoff_slide: 4, cutoff: 120
  end
  sleep 4
end
```
