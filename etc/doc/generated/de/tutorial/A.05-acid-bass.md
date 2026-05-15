A.5 Acid Bass

# Acid Bass

Wenn man sich die Geschichte der Elektronischen Tanzmusik anschaut, ist es schier unmöglich den enormen Einfluss, den der winzige Synthesizer Roland TB-303 hatte, zu übersehen. Er ist das Geheimnis hinter dem Klang des originalen Acid Bass. Diese klassisch quietschenden und quieksenden Bass Riffe des TB-303 kann man sowohl in der frühen Chicago House Szene als auch bei Interpreten moderner Elektronischen Musik wie Plastikman, Squarepusher und Aphex Twin hören.

Interessanterweise hatte Roland nie vorgesehen, dass der TB-303 für Tanzmusik zum Einsatz kommt. Er wurde ursprünglich als eine Übungshilfe für Gitarristen entwickelt. Die Firma hatte sich überlegt, dass Leute mit Hilfe des Synthesizers Basslinien programmieren würden zu denen sie jammen könnten. Leider gab es damit mehrere Probleme: die Programmierung war etwas zu kompliziert, der Klang glich nicht wirklich einer Bassgitarre, und sie waren teuer in der Anschaffung. Um ihre Verluste zu minimieren, beendete Roland die Produktion nach dem Verkauf von 10.000 Exemplaren. Nach ein paar Jahren des Daseins als Staubfänger in den Regalen der Gitarristen wanderten die meisten TB-303s in die Schaufenster von Second-Hand-Läden. Dort warteten sie auf ihre Wiederentdeckung durch eine neue Generation von Experimentierfreudigen, die anfingen sie auf eine Art und Weise zu nutzten, die Roland nie vorgesehen hatte, um abgefahrene Klänge zu erzeugen. Das war die Geburtsstunde des Acid House.

Obwohl es nicht leicht ist sich einen originalen TB-303 zu beschaffen, wirst Du Dich sicher freuen zu hören, dass Du Deinen Raspberry Pi mit Hilfe von Sonic Pi in einen TB-303 verwandeln kannst. Los gehts, wirf Sonic Pi an, kopiere den folgenden Code in einen leeren Puffer und klicke auf Ausführen:

```
use_synth :tb303
play :e1
```
    
Siehe da! Wir haben einen Acid Bass! Lass uns weiter experimentieren…

## Bringe den Bass zum Glucksen

Lass uns zunächst einen live Arpeggiator bauen. In unserer letzten Tutorialeinheit haben wir gelernt, dass Riffs durch Ringe von Noten, die wir nach einander in einer sich wiederholenden Schleife wiedergeben, repräsentiert werden können. Lass uns einen Live-Loop schreiben, der das gleiche macht:

```
use_synth :tb303
live_loop :squelch do
  n = (ring :e1, :e2, :e3).tick
  play n, release: 0.125, cutoff: 100, res: 0.8, wave: 0
  sleep 0.125
end
```

Schaue dir jede Codeziele genau an.

1. In der ersten Zeile setzen wir den Defaultwert der`use_synth` Funktion auf `tb303`.
2. In der zweiten Zeile erstellen wir einen Live-Loop vom Typen `:squelch`, der sich die ganze Zeit wiederholt.
3. In der dritten Zeile erstellen wir unseren Riff - einen Ring aus Noten (E in den Oktaven 1, 2, und 3), der diese mit Hilfe eines `.tick`s in Schleife durchläuft. Wir definieren `n` als die aktuelle Note in unserem Riff. Das Gleichheitszeichen hier bedeutet, dass wir den Wert auf der rechten Seite der Bezeichnung auf der linken Seite zuzuweisen. In jedem Durchlauf unseres Loops hat `n`also ein anderer Wert. Im ersten Durchlauf wird `n` auf `:e1` gesetzt. Im zweiten Durchlauf wird es auf `:e2`, gefolgt von `:e3` und dann wieder auf `:e1` usw. gesetzt.
4. Mit Zeile viert wird der eigentliche `:tb303` Synth eingeleitet. Dabei werden einige interessante Optionen mitgegeben: `release:`, `cutoff:`, `res:` and `wave:` über die wir etwas später genauer sprechen werden.
5. In Zeile fünf machen wir eine Pause - wir geben unserem Live-Loop vor sich alle `0.125`s (oder 8 mal pro Sekunde bei einem BPM von 60) zu wiederholen.
6. Zeile sechs markiert das Ende (`end`) des Live-Loops. Sie teilt Sonic Pi nur mit, wo die zu wiederholende Sequenz zu Ende ist.

Während du dir noch überlegst, wie das alles genau funktioniert, tippe den oben stehenden Code ab und klicke auf Ausführen. Du solltest den `:tb303` lostreten hören. Jetzt legen wir richtig los: lass uns mit dem Live-Coding beginnen.

Während der Loop noch aktiv ist, ändere den `cutoff:`-Wert auf `110`. Drücke nun erneut den „Ausführen“-Knopf. Der Ton sollte sich nun etwas härter und quäkiger anhören. Ändere ihn jetzt auf `120` und drücke „Ausführen“. Jetzt `130`. Hörst du, wie höhere Cutoff-Werte den Klang durchdringender und intensiver machen? Ändere den Wert schließlich auf `80`, wenn du dich nach einer Pause sehnst. Wiederhole das Ganze dann so oft wie du willst. Keine Sorge, ich werde immer noch hier sein...

Eine weitere Option, mit der es sich zu spielen lohnt, ist `res:`. Damit wird der Resonanzgrad des Filters eingestellt. Eine hohe Resonanz ist charakteristisch für Acid-Bass-Sounds. Im Moment haben wir `res:` auf `0.8` eingestellt. Versuche, ihn auf `0.85`, dann `0.9` und schließlich `0.95` zu erhöhen. Möglicherweise wirst du feststellen, dass ein Cutoff-Wert wie `110` oder höher die Unterschiede besser hörbar macht. Jetzt mach mal etwas wildes und gib `0.999` ein: hörst du den verrückten Sound? Bei einer so hohen Auflösung hört man den Cutoff-Filter so stark mitschwingen, dass er anfängt, eigene Töne zu erzeugen!

Ändere zum Schluss die `wave:` Option auf `1`, um einen großen Einfluss auf die Klangfarbe zu nehmen. Dies legt die Art der Schwingungserzeugung fest. Der Standard ist mit `0` eine Sägezahnschwingung. `1` steht für eine Pulswelle und `2`für eine Dreiecksschwingung.

Versuche natürlich auch verschiedene Riffs zu erzeugen, indem du die Noten im Ring änderst oder sogar Noten aus Skalen oder Akkorden auswählst. Viel Spaß mit deinem ersten Acid Bass Synth.

## Den TB-303 zerlegen

Der Aufbau des originalen TB-303 ist eigentlich ganz einfach. Wie man dem folgenden Diagramm entnehmen kann, gibt es nur vier Hauptbestandteile.

![TB-303 Design](../../../etc/doc/images/tutorial/articles/A.05-acid-bass/tb303-design.png)

Zunächst haben wir die Oszillatorwelle – die Rohzutaten des Klangs. In diesem Fall haben wir eine Rechteckwelle. Als nächstes gibt es die Amplitudenhüllkurve des Oszillators, die den Verstärker der Rechteckwelle über die Zeit steuert. Der Zugriff auf diese erfolgt in Sonic Pi über die Optionen „attack:“, „decay:“, „sustain:“ und „release:“ zusammen mit ihren Level-Gegenstücken. Weitere Informationen finden Sie in Abschnitt 2.4 „Dauer mit Hüllkurven“ im integrierten Tutorial. Anschließend leiten wir unsere umhüllte Rechteckwelle durch einen resonanten Tiefpassfilter. Dadurch werden die höheren Frequenzen abgehackt und es entsteht ein schöner Resonanzeffekt. Jetzt beginnt der Spaß. Der Cutoff-Wert dieses Filters wird auch durch seine eigene Hüllkurve gesteuert! Das bedeutet, dass wir durch das Spielen mit beiden Hüllkurven eine erstaunliche Kontrolle über die Klangfarbe haben. Werfen wir einen Blick darauf:

```
use_synth :tb303
with_fx :reverb, room: 1 do
  live_loop :space_scanner do
    play :e1, cutoff: 100, release: 7, attack: 1, cutoff_attack: 4, cutoff_release: 4
    sleep 8
  end
end
```
    
Für jede Standard-Hüllkurvenoption gibt es eine entsprechende `cutoff_`-Option im `:tb303`-Synthesizer. Um die cutoff-Anschlagszeit zu ändern, können wir die Option `cutoff_attack:` verwenden. Kopiere den Code unten in einen leeren Puffer und drücke *Run*. Du wirst einen verrrückten Sound hören, der ein- und auswobbelt. Nun spiel ein bisschen herum. Versuche, die `cutoff_attack:`-Zeit auf `1` zu ändern, dann auf `0.5`. Und jetzt probiere `8`.

Beobachte, dass wir, um etwas mehr Stimmung zu erzeugen, alles durch einen `:reverb` FX schicken - probiere ein paar andere Effekte aus und schaue welche hier gut passen!

## Alles zusammenführen

Zum Schluss gibt es ein Beispiel, das ich mit Hilfe der Konzepte aus diesem Tutorial komponiert habe. Kopiere den Code in einen leeren Puffer, höre es Dir ein Weile lang an und dann versuche Deine eigenen Änderungen live zu coden. Überzeuge Dich davon, was für verrückte Klänge Du erzeugen kannst! Bis zum nächste Mal …

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
