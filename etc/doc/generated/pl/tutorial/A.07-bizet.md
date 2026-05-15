A.7 Bizet'owe Bity

# Bizet'owe Bity

Po naszej krótkiej wycieczce do fantastycznego świata kodowania Minecraft'a z Sonic Pi w ubiegłym miesiącu wróćmy z powrotem do muzyki. Dzisiaj przeniesiemy klasyczną operowy taniec prosto w XXI wiek, używając niesamowitej potęgi kodu.

## Skandaliczne i Szkodliwe

Wskoczmy do wehikułu czasu i cofnijmy się do roku 1875. Kompozytor o nazwisku Bizet właśnie skończył jego najnowszą operę Carmen. Niestety, jak to ma zazwyczaj miejsce z wieloma nowymi ekscytującymi utworami, ludzie na początku nie polubili jej wcale, ponieważ była zbyt skandaliczna i inna niż wszystko. Niestety Bizet zmarł dziesięć lat przed jej wielkim międzynarodowym sukcesem. Opera stała się jedną z najsławniejszych i najczęściej granych oper wszech czasów. Z współczuciem dla tej tragedii weźmy jeden z najbardziej znanych motywów z Carmen i przekształćmy go do nowoczesnego formatu muzyki, który również będzie zbyt skandaliczny i inny dla większości ludzi w naszych czasach - muzykę kodowaną na żywo!

## Rozszyfrowywanie Habanery

Próba kodowania na żywo całej opery mogłaby być sporym wyzwaniem dla tego poradnika, skupmy się więc na jednej z jej najbardziej popularnych części - sekcja basowa Habanery:

![Habanera Riff](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera.png)

Jeśli nie uczyłeś się jeszcze notacji muzycznej, to może to dla Ciebie wyglądać ekstremalnie niezrozumiałe. Jednakże, jako programiści, widzimy notację muzyczną jako kolejną formę kodu - jedyna różnica polega tu na tym, że reprezentuje instrukcje dla muzyka zamiast komputera. Musimy zatem znaleźć sposób na jego rozszyfrowanie go.

## Nuty

Nuty są ułożone od lewej do prawej tak samo jak słowa, w tym tekście mają jednak różne wysokości. *Wysokość na partyturze reprezentuje wysokość danej nuty.* Im wyżej nuta znajduje się na pięciolinii, tym wyższy jest dźwięk tej nuty.

Wiemy już w jaki sposób możemy zmieniać wysokość nut w Sonic Pi - możemy używać dużych lub małych liczb takich jak `play 75` czy `play 80`. Możemy też użyć nazw nut np. `play :E` lub `play :F`. Na szczęście każda z pozycji na pięciolinii reprezentuje konkretną nazwę nuty. Zerknij na tą przydatną tabelkę poniżej:

![Notes](../../../etc/doc/images/tutorial/articles/A.07-bizet/notes.png)

## Przerwy

Nuty są bardzo bogatym i ekspresyjnym sposobem na przedstawianie wielu rzeczy. Skoro tak, to nie powinno Cię zbytnio zdziwić fakt, że nuty potrafią powiedzieć nie tylko jakie nuty powinniśmy zagrać, ale też kiedy ich *nie* grać. W programowaniu jest to bardzo analogiczne do idei `nil` lub `null` - oznacza ona brak wartości. Innymi słowy, moment, w którym nie gramy nuty, jest miejscem, gdzie jej nie ma.

Jeśli przyjrzysz się uważnie nutom, zauważysz, że jest ona aktualnie kombinacją czarnych kropek z liniami reprezentujące nuty do zagrania oraz falujące ogonki, które oznaczają przerwy. Na nasze szczęście Sonic Pi posiada bardzo poręczną reprezentację dla przerw - `:r`. Jeśli więc uruchomimy polecenie `play :r`, to zagra on ciszę! Możemy napisać również `play :rest`, `play nil` lub `play false`. Są to analogiczne odpowiedniki dla reprezentacji przerw.

## Rytm

Na końcu jest jeszcze jedna rzecz do zrozumienia przy rozszyfrowywaniu nut - czasy ich trwania. Zauważ, że w oryginalnej notacji niektóre są połączone grubymi liniami, które nazywamy belkami. Druga nuta posiada dwie takie belki, co oznacza, że przerwa trwa jedną szesnastą uderzenia (bitu). Inne nuty posiadają pojedyncze belki - trwają one przez jedną ósmą uderzenia. Pozostałe nuty mają dwie falowane belki - one również reprezentują jedną szesnastą uderzenia.

Kiedy próbujemy rozszyfrować i odkrywać nowe rzeczy, bardzo przydatną sztuczką jest robienie wszystkiego tak bardzo analogicznie, jak to jest tylko możliwe, aby móc spróbować i wybadać ew. relacje lub wzorce. Na przykład, kiedy przepisujemy szesnastki, możesz zobaczyć, że nasza notacja zostaje zmieniona w prostą sekwencję nut i przerw.

![Habanera Riff 2](../../../etc/doc/images/tutorial/articles/A.07-bizet/habanera2.png)

## Zamiana Habanery na kod

Jesteśmy teraz w stanie zacząć tłumaczyć tę linię basową na kod zrozumiały dla Sonic Pi. Spróbujmy rozszyfrować te noty i przerwy, używając do tego celu pierścienia:

```
(ring :d, :r, :r, :a, :f5, :r, :a, :r)
```
    
Zobaczmy, jak to brzmi. Wrzuć ten kawałek kodu do żywej pętli i zacznij przez niego tykać:

```
live_loop :habanera do
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
```
    
Fantastycznie, ten natychmiastowo rozpoznawalny riff został przywrócony do życia w Twoich głośnikach. Dotarcie tutaj kosztowało wiele wysiłku, ale opłaciło się - przybij piątkę!
    
## Nastrojowe Syntezatory

Teraz, gdy już mamy linię basową, spróbujmy odtworzyć klimat sceny operowej. Jednym z syntezatorów do wypróbowania to`:blade`, który jest jednym z wiodących w stylu lat 80-tych. Spróbujmy go użyć z początkową nutą `:d` i przepuścić przez efekty slicer i reverb:

```
live_loop :habanera do
  use_synth :fm
  use_transpose -12
  play (ring :d, :r, :r, :a, :f5, :r, :a, :r).tick
  sleep 0.25
end
with_fx :reverb do
  live_loop :space_light do
    with_fx :slicer, phase: 0.25 do
      synth :blade, note: :d, release: 8, cutoff: 100, amp: 2
    end
    sleep 8
  end
end
```

A teraz spróbuj z innymi nutami z naszej linii basowej: `:a` i `:f5`. Pamiętaj - nie musisz naciskać stop - wystarczy, że zmodyfikujesz kod, podczas gdy muzyka wciąż gra i naciśniesz przycisk Run. Spróbuj również ustawić inne wartości dla opcji `phase:` np. `0.5`, `0.75` czy `1`.

## Połączenie tego wszystkiego w jedną całość

Na koniec spróbujmy połączyć wszystkie pomysły, tak aby stworzyć nowy remix Habanery. Możesz zauważyć, że dorzuciłem kolejną partię linii basowej jako komentarz. Gdy już wpiszesz to wszystko do świeżego buforu, naciśnij przycisk Run, żeby usłyszeć kompozycję. Teraz, bez naciskania przycisku Stop, *odkomentuj* drugą linię poprzez usunięcie znaku `#` i ponownie wciśnij Run - ależ to jest fenomenalne! A teraz zacznij kombinować z tym kodem samodzielnie i baw się dobrze.

```
use_debug false
bizet_bass = (ring :d, :r, :r, :a, :f5, :r, :a, :r)
#bizet_bass = (ring :d, :r, :r, :Bb, :g5, :r, :Bb, :r)
 
with_fx :reverb, room: 1, mix: 0.3 do
  live_loop :bizet do
    with_fx :slicer, phase: 0.125 do
      synth :blade, note: :d4, release: 8,
        cutoff: 100, amp: 1.5
    end
    16.times do
      tick
      play bizet_bass.look, release: 0.1
      play bizet_bass.look - 12, release: 0.3
      sleep 0.125
    end
  end
end
 
live_loop :ind do
  sample :loop_industrial, beat_stretch: 1,
    cutoff: 100, rate: 1
  sleep 1
end
 
live_loop :drums do
  sample :bd_haus, cutoff: 110
  synth :beep, note: 49, attack: 0,
    release: 0.1
  sleep 0.5
end
```
