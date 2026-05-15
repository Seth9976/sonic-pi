11 MIDI

# MIDI

Gdy opanujesz konwertowanie kodu na muzykę, możesz się zastanawiać – co dalej? Czasami ograniczenia wynikające z pracy wyłącznie ze składnią i systemem dźwiękowym Sonic Pi mogą być ekscytujące i wpłynąć na nową kreatywność. Jednakże czasami istotne jest, aby wyjść poza kod i zanurzyć się w prawdziwym świecie. Chcemy uzyskać dwie dodatkowe rzeczy:

1. Aby móc przekształcać zdarzenia z prawdziwego świata na zdarzenia w Sonic Pi, które można zakodować
2. To be able to use Sonic Pi's strong timing model and semantics to control and manipulate objects in the real world

Luckily there's a protocol that's been around since the 80s that enables exactly this kind of interaction - MIDI. There's an incredible number of external devices including keyboards, controllers, sequencers, and pro audio software that all support MIDI. We can use MIDI to receive data and also use it to send data.

Sonic Pi provides full support for the MIDI protocol enabling you to connect your live code to the real world. Let's explore it further...
