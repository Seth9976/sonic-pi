A.6 Musisches Minecraft

# Musisches Minecraft


Minecraft Pi


Hallo und willkommen zurück! In den zurückliegenden Tutorialeinheiten haben wir uns ausschließlich auf die musikalischen Möglichkeiten von Sonic Pi konzentriert (um unseren Raspberry Pi in ein konzertfähiges Musikinstrument zu verwandeln). Bisher haben wir gelernt wie man:

* Live-Coded - Klänge live improvisieren,
* gigantische Beats komponiert,
* mächtige Lead Synths generiert,
* und den bekannten TB-303 Acid Bass nachbaut.

Es gibt noch so viel Dinge zu entdecken (was wir in zukünftigen Ausgaben des Tutorials auch machen werden). Diesen Monat lass uns einen Blick auf etwas werfen, was Sonic Pi kann, das du wahrscheinlich nicht erwartet hast: Minecraft steuern.

## Hello Minecraft World

OK, lass uns loslegen. Fahre deinen Raspberry Pi hoch, werfe Minecraft Pi an und erstelle eine neue Welt. Starte nun Sonic Pi und verändere die Größe deiner Fenster so, dass du sowohl Sonic Pi als auch Minecraft Pi auf deinem Bildschirm sehen kannst.

Gib Folgendes in einen leeren Puffer ein:

```
mc_message "Hello Minecraft from Sonic Pi!"
```
    
Drücke jetzt auf Ausführen. Boom! Deine Nachricht erscheint in Minecraft! Wie einfach war das denn? Lege nun dieses Tutorial kurz beiseite und spiele mit deinen eigenen Nachrichten herum. Viel Spaß!

![Bildschirm 0](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-0-small.png)

## Schall-Teleporter

Lass uns ein wenig erkunden. Die üblichste Option ist Maus und Tastatur zu ergreifen und einfach loszulaufen. Das funktioniert, ist aber langsam und langweilig. Es wäre doch viel besser, wenn wir eine Art Teleporter hätten. Dank Sonic Pi haben wir diesen. Probier dieses:

```
mc_teleport 80, 40, 100
```
    
Meine Güte! Das war ein langer Weg nach oben. Wenn du dich nicht im Flugmodus befunden hättest, wärst du den ganzen Weg zurück auf den Boden gefallen. Wenn du doppelt auf die Leertaste drückst, um in den Flugmodus zu wechseln und dich wieder zu teleportieren, schwebst du weiterhin an der Stelle, an die du dich teleportiert hast.

Aber was bedeuten diese Zahlen? Wir haben drei Zahlen, welche die Koordinaten beschreiben, zu denen wir in der Welt gehen wollen. Jede der Zahlen bekommt einen Namen - x, y und z:

* x - wie weit links und rechts (80 in unserem Beispiel)
* y - wie hoch wir sein wollen (40 in unserem Beispiel)
* z - wie weit vorwärts und rückwärts (100 in unserem Beispiel)

Indem wir unterschiedliche Werte für x, y und z wählen, können wir uns an *jeden* Ort der Welt teleportieren. Probiere es aus! Wähle verschiedene Zahlen aus und schaue, wo du landest. Falls der Bildschirm schwarz wird, hast du dich in den Boden oder in einen Berg teleportiert. Wähle in diesem Fall einen höheren y-Wert um wieder über den Boden zu kommen. Erkunde weiter, bis du einen Platz findest, der dir gefällt...

Using the ideas so far, let's build a Sonic Teleporter which makes a fun teleport sound whilst it whizzes us across the Minecraft world:

```
mc_message "Preparing to teleport...."
sample :ambi_lunar_land, rate: -1
sleep 1
mc_message "3"
sleep 1
mc_message "2"
sleep 1
mc_message "1"
sleep 1
mc_teleport 90, 20, 10
mc_message "Whoooosh!"
```
    
![Bildschirm 1](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-1-small.png)

## Magische Blöcke

Now you've found a nice spot, let's start building. You could do what you're used to and start clicking the mouse furiously to place blocks under the cursor. Or you could use the magic of Sonic Pi. Try this:

```
x, y, z = mc_location
mc_set_block :melon, x, y + 5, z
```

Now look up! There's a melon in the sky! Take a moment to look at the code. What did we do? On line one we grabbed the current location of Steve as the variables x, y and z. These correspond to our coordinates described above. We use these coordinates in the fn `mc_set_block` which will place the block of your choosing at the specified coordinates. In order to make something higher up in the sky we just need to increase the y value which is why we add 5 to it. Let's make a long trail of them:

```
live_loop :melon_trail do
  x, y, z = mc_location
  mc_set_block :melon, x, y-1, z
  sleep 0.125
end
```

Now, jump over to Minecraft, make sure you're in flying-mode (double tap space if not) and fly all around the world. Look behind you to see a pretty trail of melon blocks! See what kind of twisty patterns you can make in the sky.

## Live Coding Minecraft

Those of you that have been following this tutorial over the last few months will probably have your minds blown at this point. The trail of melons is pretty cool, but the most exciting part of the previous example is that you can use `live_loop` with Minecraft! For those that don't know, `live_loop` is Sonic Pi's special magic ability that no other programming language has. It lets you run multiple loops at the same time and allows you to change them whilst they run. They are incredibly powerful and amazing fun. I use `live_loop`s to perform music in nightclubs with Sonic Pi - DJs use discs and I use `live_loop`s :-) However, today we're going to live code both music and Minecraft.

Let's get started. Run the code above and start making your melon trail again. Now, without stopping the code, just simply change `:melon` to `:brick` and hit run. Hey presto, you're now making a brick trail. How simple was that! Fancy some music to go with it? Easy. Try this:

```
live_loop :bass_trail do
  tick
  x, y, z = mc_location
  b = (ring :melon, :brick, :glass).look
  mc_set_block b, x, y -1, z
  note = (ring :e1, :e2, :e3).look
  use_synth :tb303
  play note, release: 0.1, cutoff: 70
  sleep 0.125
end
```
    
Now, whilst that's playing start changing the code. Change the block types - try `:water`, `:grass` or your favourite block type. Also, try changing the cutoff value from `70` to `80` and then up to `100`. Isn't this fun?

## Alles zusammenführen

![Bildschirm 2](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-2-small.png)

Let's combine everything we've seen so far with a little extra magic. Let's combine our teleportation ability with block placing and music to make a Minecraft Music Video. Don't worry if you don't understand it all, just type it in and have a play by changing some of the values whilst it's running live. Have fun and see you next time...
    
```
live_loop :note_blocks do
  mc_message "This is Sonic Minecraft"
  with_fx :reverb do
    with_fx :echo, phase: 0.125, reps: 32 do
      tick
      x = (range 30, 90, step: 0.1).look
      y = 20
      z = -10
      mc_teleport x, y, z
      ns = (scale :e3, :minor_pentatonic)
      n = ns.shuffle.choose
      bs = (knit :glass, 3, :sand, 1)
      b = bs.look
      synth :beep, note: n, release: 0.1
      mc_set_block b, x+20, n-60+y, z+10
      mc_set_block b, x+20, n-60+y, z-10
      sleep 0.25
    end
  end
end
live_loop :beats do
  sample :bd_haus, cutoff: 100
  sleep 0.5
end
```
