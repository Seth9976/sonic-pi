A.3 Zakodowane Bity

# Zakodowane bity

Jednym z najbardziej ekscytujących i zawrotnych rozwoju technicznego nowoczesnej muzyki było wynalezienie samplerów - znane jako pudełka pozwalające na nagrywanie i wrzucenie do nich dowolnego dźwięku, który można było potem zmieniać i odtwarzać te dźwięki na wiele ciekawych sposobów. Na przykład mogłeś wziąć jakieś stare nagranie, znaleźć sekcję z solówką na perkusji, nagrać ją na Twoim samplerze, a następnie odtwarzać ją w kółko, zwalniając przy tym tempo tego fragmentu o połowę, aby stworzyć bazę dla Twoich najnowszych bitów. W taki właśnie sposób narodził się wczesny hip-hop i w dzisiejszych czasach prawie niemożliwym jest znalezienie muzyki elektronicznej, która by nie korzystała z sampli w jakimś stopniu. Używanie ich jest naprawdę świetnym sposobem na wprowadzanie nowych i ciekawych elementów do Twoich występów polegających na kodowaniu muzyki na żywo.

Skąd w takim razie możesz wziąć sampler? Właściwie to aktualnie już jeden posiadasz - to Twój komputer Raspberry Pi (albo Mac lub PC)! Aplikacja Sonic Pi posiada swój własny wbudowany, który jest bardzo potężny. Spróbujmy się nim pobawić!

## Amen Break

Jeden z najbardziej klasycznych i rozpoznawalnych samplem zawierającym perkusję nazywa się Amen Break. Po raz pierwszy został użyty w roku 1969 w piosence "Amen Brother" (Amen Bracie) przez zespół Winstons jako część solówki na perkusji. Jednakże, od chwili kiedy została odkryta przez wczesnych muzyków tworzących hip-hop w latach 80-tych i użyta w samplerach, zaczęła być bardzo często używana w wielu innych stylach muzycznych np. drum and bass, breakbeat, hardcore techno i breakcore.

Jestem pewien, że będziesz podekscytowany, gdy powiem Ci, że one też są wbudowane w Sonic Pi. Wyczyść bufor i wklej następujący kod:

```
sample :loop_amen
```

Naciśnij *Run* i bum! Słuchasz jednej z najbardziej wpływowych połamanych pętli w historii muzyki tanecznej. Jednakże sampel ten nie zawdzięcza swej sławy jednorazowemu odtworzeniu - został stworzony po to, aby kręcić się w kółko.


## Sięgnięcie Po Beat

Zapętlijmy amen break, używając naszego starego dobrego przyjaciela `live_loop`, wprowadzonego w tym poradniku ostatniego miesiąca:

```
live_loop :amen_break do
  sample :loop_amen
  sleep 2
end
```

Okej, więc to jest zapętlone, ale są tutaj irytujące pauzy za każdym razem, gdy ponownie odtwarzany jest sampel. Tak się dzieje, gdyż poprosiliśmy go o uśpienie na '2' uderzenia, a z domyślnym BPM 60. `: loop_amen` trwa jedynie przez `1.753` uderzenia. Mamy więc ciszę przez 0.247 uderzenia ('2-1.753 = 0.247'). Mimo krótkiego czasu trwania, jest to jeszcze zauważalne.

Żeby naprawić ten problem, możemy użyć opcji `beat_stretch:`, aby poprosić Sonic Pi, by rozciągnęło (lub skurczyło) sampel, tak aby wpasował się w określoną liczbę uderzeń (bitów).

Funkcje `sample` i `synth`, które są dostępne w Sonic Pi, pozwalają Ci na dużą kontrolę za pomocą opcjonalnych parametrów takich jak `amp:`, `cutoff:` czy `release:`. Jednakże termin parametry opcjonalne jest dosyć długi, więc będziemy używać skróconego terminu *opcje*, aby było nam wygodniej i łatwiej.

```
live_loop :amen_break do
  sample :loop_amen, beat_stretch: 2
  sleep 2
end  
```

Teraz możemy zacząć tańczyć! Aczkolwiek może chcielibyśmy przyśpieszyć trochę tempo lub zwolnić, aby dostosować je do nastroju.

## Zabawa z Czasem

Dobrze! Więc co, jeśli chcemy zmienić styl na starą szkołę hip-hop lub breakcore? Jednym z prostszych sposobów jest zabawa z czasem - lub, w innym słowa znaczeniu, bałagan z tempem. Możesz z tego korzystać w Sonic Pi w bardzo łatwy sposób - po prostu dodaj 'use_bpm' do Twojej żywej pętli:

```
live_loop :amen_break do
  use_bpm 30
  sample :loop_amen, beat_stretch: 2
  sleep 2
end 
```

Zauważ, że w czasie, gdy rapujesz do tych powolnych uderzeń, przy każdym kolejnym obiegu pętli wciąż śpimy przez 2 uderzenia, nasze BPM (z ang. Beats Per Minute - ilość uderzeń na minutę) jest równe 30, a wszystkie dźwięki idealnie mieszczą się w czasie jednej pętli. Opcja `beat_stretch` współpracuje z aktualnym BPM, aby zapewnić, że wszystko działa tak, jak należy.

A teraz czas na zabawę. Podczas gdy pętla jest wciąż żywa, zmień wartość `30` w linijce `use_bpm 30` na `50`. Łoooo, wszystko ot tak przyśpiesza, *a jednocześnie* wciąż mieści się w czasie trwania jednej pętli! Spróbuj pójść jeszcze szybciej - do 80, do 120, a nawet zaszalej i uderz w 200!


## Filtrowanie

Potrafimy już wplatać sample do naszych żywych pętli, spójrzmy zatem na jedną z najfajniejszych opcji dostępnych dla syntezatora `sample`. Pierwszą jest `cutoff:`, która kontroluje filtr odcięcia samplera. Domyślnie jest ona wyłączona, ale możesz ją bardzo łatwo włączyć:

```
live_loop :amen_break do
  use_bpm 50
  sample :loop_amen, beat_stretch: 2, cutoff: 70
  sleep 2
end  
```

Śmiało, spróbuj zmienić wartość opcji `cutoff:`. Na przykład zwiększ ją do 100, naciśnij przycisk *Run*, poczekaj, aż aktualna pętla skończy swój przebieg, aby usłyszeć zmianę w dźwięku. Zauważ, że niskie wartości takie jak 50 brzmią łagodnie i basowo, natomiast wyższe wartości takie jak 100 czy 120 pozwalają uzyskać pełniejsze i bardziej zgrzytliwe brzmienie. Dzieje się tak ponieważ opcja `cutoff:` wycina część dźwięku o wyższych częstotliwościach - tak jak kosiarka ścina trawę. Opcja `cutoff:` jest jak ustawienie długości - określamy, jaka wysokość trawy ma pozostać po skoszeniu.


## Przycinanie

Kolejnym świetnym narzędziem do zabawy jest efekt slicer. Użycie go spowoduje posiekanie dźwięku (z ang. slice - pokroić). Opakujmy linijkę zawierającą polecenie `sample` w efekt tak jak na poniższym kodzie:

```
live_loop :amen_break do
  use_bpm 50
  with_fx :slicer, phase: 0.25, wave: 0, mix: 1 do
    sample :loop_amen, beat_stretch: 2, cutoff: 100
  end
  sleep 2
end
```

Zauważ, jak dźwięki odbijają się po trochu w górę i w dół. (Możesz usłyszeć pierwotne brzmienie bez efektu, zmieniając wartość opcji `mix:` na `0`.) Teraz spróbuj pobawić się opcją `phase:`. Opowiada ona za tempo (w uderzeniach), z jakim jest aplikowany efekt cięcia. Mniejsze wartości takie jak `0.125` spowodują, że cięcia będą szybsze, natomiast większe wartości takie jak `0.5` będą powodować, że staną się wolniejsze. Zauważ, że zmniejszenie lub zwiększenie o połowę wartości opcji `phase:` zazwyczaj powoduje fajne brzmienia. Na koniec zmień wartość opcji `wave:` na 0, 1 lub 2, a usłyszysz, jak zmienia się dźwięk. To są różne barwy dźwięku. 0 to fala piłokształtna (mocne wejście i normalne wyjście), 1 to fala kwadratowa (mocne wejście, mocne wyjście), natomiast 2 to fala trójkątna (normalne wejście, normalne wyjście).


## Połączenie tego wszystkiego w jedną całość

Na koniec cofnijmy się w czasie i zanurzmy się ponownie we wczesnej scenie drum and bass miasta Bristol wykorzystując do tego celu przykład z bieżącego miesiąca. Nie przejmuj się zbytnio tym, że nie rozumiesz wszystkiego z tego kawałka kodu, po prostu przepisz go lub przekopiuj i naciśnij przycisk Run, a następnie zacznij kodować na żywo, zmieniając liczby znajdujące się przy opcjach i zobacz, dokąd Cię to zaprowadzi. Pamiętaj tylko, żeby podzielić się tym, co uda Ci się stworzyć! Do zobaczenia następnym razem...

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
