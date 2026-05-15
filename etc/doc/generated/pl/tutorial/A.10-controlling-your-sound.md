A.10 Kontrola

# Kontroluj Swój Dźwięk

Do tej pory skupialiśmy się w tej serii na tworzeniu dźwięków. Odkryliśmy, że możemy uruchamiać wiele różnych syntezatorów wbudowanych w Sonic Pi z wykorzystaniem poleceń `play` i `synth` oraz wiemy, w jaki sposób uruchamiać nagrane wcześniej sample z wykorzystaniem polecenia `sample`. Zobaczyliśmy również, że możemy na te dźwięki nałożyć efekty studyjne (FX) takie jak reverb czy distortion, wykorzystując do tego polecenie `with_fx`. Jeśli połączysz to wszystko z niesamowicie dokładnym systemem koordynacji czasu (timing system), jaki posiada Sonic Pi, to daje Ci to szeroki wachlarz dźwięków, bitów i riff'ów. Jednakże gdy raz uruchomisz te pieczołowicie dobierane brzmienia, to nie ma już możliwości, aby cokolwiek zmienić, gdy muzyka już gra, prawda? Błąd! Dzisiaj nauczysz się czegoś bardzo potężnego - jak kontrolować syntezatory, gdy te już wystartowały.

## Podstawowy Dźwięk

Stwórzmy prosty, ładny dźwięk. Uruchom Sonic Pi oraz w wolnym buforze wpisz poniższy tekst:

```
synth :prophet, note: :e1, release: 8, cutoff: 100
```

Naciśnij teraz przycisk *Run*, który znajduje się w lewym górnym rogu, by następnie usłyszeć piękny, dudniący dźwięk syntezatora. Śmiało, zrób to jeszcze kilka razy - poczuj ją. Okej, zrobione? Zacznijmy ją kontrolować!

## Węzły Syntezatorów (Synth Nodes)

Małą znaną funkcją w Sonic Pi jest to, że polecenia `play`, `synth` oraz `sample` zwracają coś, co nazywamy `SynthNode`. Obiekt ten reprezentuje grający dźwięk. Możesz przechwycić jeden z tych `SynthNode`ów, korzystając ze standardowego mechanizmu zmiennych, a następnie **kontrolować** go w dalszym punkcie czasu. Przykładowo zmieńmy wartość opcji `cutoff:` po jednym uderzeniu:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
control sn, cutoff: 130
```

Spójrzmy na każdy wiersz:

Najpierw uruchamiamy syntezator `:prophet`, korzystając tak jak zwykle z funkcji `synth`. Jednakże przechwytujemy tutaj także wynik tego wywołania i zapisujemy go w zmiennej `sn`. Moglibyśmy nazwać tą zmienną zupełnie inaczej, np. `synth_node` lub `jane` - ta nazwa tak naprawdę nie ma znaczenia. Lecz bardzo ważne jest to, aby wybierać takie nazwy, które będą zrozumiałe i pomocne dla Ciebie podczas Twoich występów oraz dla ludzi czytających Twój kod. Wybrałem nazwę `sn`, ponieważ jest to bardzo fajny skrót do zapamiętania dla węzła syntezatora (synth node).

W linii drugiej mamy podstawową komendę `sleep`. Nie robi ona nic specjalnego - tylko pyta komputer o czekanie na jedno uderzenie przed przejściem do następnej linii.

Linia trzecia to miejsce, gdzie rozpoczyna się zabawa z kontrolą. Tutaj używamy funkcji `control`, by powiedzieć naszemu działającemu `SynthNode` o zmianie wartości na `130`. Kiedy naciśniesz przycisk **Run**, usłyszysz, że syntezator `:prophet` gra jak wcześniej, jednak po jednym uderzeniu zmieni dźwięk na znacznie jaśniejszy.

Opcje Z Możliwością Modulacji

Większość opcji syntezatorów i efektów (FX) w Sonic Pi może zostać zmieniona po uruchomieniu. Jednakże nie należą do nich wszystkie. Na przykład opcje `attack:`, `decay:`, `sustain:` i `release:` dotyczące obwiedni dźwięku mogą być ustawione tylko raz przy uruchomieniu dźwięku. Ocena tego, czy dana opcja może lub nie może być zmieniana, jest prosta - wystarczy otworzyć dokumentację dla danego syntezatora lub efektu, a następnie przewinąć ją w dół do szczegółowej dokumentacji dla każdej z opcji i poszukać fraz "May be changed whilst playing" (dana opcja może być zmieniana w trakcie gry) lub "Can not be changed once set" (nie można jej zmieniać po ustawieniu wartości). Przykładowo dokumentacja dla opcji `attack:` syntezatora `:beep` jasno mówi, że nie ma możliwości jej zmiany:

* Domyślnie: 0
* Wartość musi wynosić 0 lub być wyższa
* Nie może być zmienione po ustawieniu
* Skalowanie z bieżącą wartością BPM

## Wielokrotne Zmiany

Kiedy syntezator jest uruchomiony, nie masz żadnych ograniczeń, by zmienić to tylko raz - towarzyszy Ci wolny wybór, więc możesz go przemieniać tyle razy, ile masz na to ochotę. Dla przykładu możemy to zrobić z naszym `:prophet` na mini arpeggiator:

```
notes = (scale :e3, :minor_pentatonic)
sn = synth :prophet, note: :e1, release: 8, cutoff: 100
sleep 1
16.times do
  control sn, note: notes.tick
  sleep 0.125
end
```

W tym kawałku kodu dodaliśmy tylko kilka dodatkowych rzeczy. Najpierw zdefiniowaliśmy nową zmienną o nazwie `notes` zawierającą nuty, przez które będziemy chcieli iterować (arpeggiator to tylko fantazyjna nazwa dla czegoś, co iteruje przez listę nut w określonym porządku). Następnie zastąpiliśmy nasze pojedyncze wywołania polecenia `control` wywołaniem go 16 razy w pętli. Przy każdym uruchomieniu polecenia `control`, tykamy poleceniem `:tick` przez nasz pierścień z nutami `notes`, powtarzając go od nowa za każdym razem, gdy dobrniemy do jego końca (dzięki fantastycznym możliwościom pierścieni dostępnych w Sonic Pi). Aby wprowadzić trochę urozmaicenia, spróbuj zamiast polecenia `:tick` użyć polecenia `:choose` i zobacz, czy słyszysz różnicę.

Zauważ, że możemy zmienić wiele z nich jednocześnie. Spróbuj to zrobić z linią kontrolną i posłuchaj dla różnicy:

```
control sn, note: notes.tick, cutoff: rrand(70, 130)
```

## Przesuwne

Kiedy kontrolujemy `SynthNode`, ten odpowiada dokładnie na czas i natychmiast zmienia wartość opt na nową, niczym wciśnięty przycisk lub śmigający przełącznik odpowiadający na zmianę. Może on brzmieć rytmicznie i perkusyjnie, zwłaszcza gdy opt kieruje aspektem barwy takim jak `cutoff:`. Jednakże czasami nie chcesz zmiany, która dzieje się od razu. Zamiast tego chciałbyś płynnie przejść z obecnej wartości na nową, jakbyś poruszał suwakiem lub pokrętłem. Oczywiście Sonic Pi może to zrobić, używając `_slide:`.

Każda opcja, która może być modyfikowana, posiada również specjalną odpowiadającą jej opcję `_slide`, która pozwala Ci na ustawienie czasu ślizgania. Na przykład opcja `amp:` posiada `amp_slide:`, a opcja `cutoff:` ma `cutoff_slide`. Te opcje ślizgania działają nieco inaczej niż wszystkie inne, ponieważ mówią one nucie syntezatora, jak powinny się zachowywać, **gdy zostaną kontrolowane następnym razem**. Spójrz na to:

```
sn = synth :prophet, note: :e1, release: 8, cutoff: 70, cutoff_slide: 2
sleep 1
control sn, cutoff: 130
```

Zauważ, że ten przykład jest prawie taki sam jak poprzedni, z tą jednak różnicą, że dodaliśmy opcję `cutoff_slide:`. Powoduje to, iż następnym razem syntezator ten, będzie miał kontrolowaną opcję `cutoff:`, przejście z aktualnej wartości do kolejnej zajmie 2 uderzenia. Dlatego, kiedy użyjemy polecenia `control`, możesz usłyszeć, że odcięcie (`cutoff`) ślizga się od 70 do 130. Nadaje to temu dźwiękowi bardzo ciekawy i dynamiczny charakter. A teraz spróbuj zmienić czas trwania ślizgu `cutoff_slide:` na mniejszą wartość, np. 0.5 albo na większą, np. 4, aby zobaczyć, jak zmienia się brzmienie. Pamiętaj, że możesz ślizgać się w ten sposób po każdej modyfikowalnej opcji, a każda wartość opcji `_slide:` może być całkowicie inna. Możesz więc ślizgać się powoli po filtrze odcięcia (`cutoff:`), amplituda dźwięku (`amp:`) może przesuwać się szybko, a zmiana kanału z jednego na drugi (`pan:`) może być gdzieś pomiędzy, o ile jest to brzmienie, którego właśnie szukasz...

## Połączenie tego wszystkiego w jedną całość

Rzućmy okiem na krótki przykład, który demonstruje moc kontrolowania syntezatorów po tym, jak zostały one uruchomione. Zauważ, że możesz także przesuwać FX jak syntezatory, choć korzystając z trochę innej składni. Sprawdź sekcję 7.2 we wbudowanym poradniku, by uzyskać więcej informacji na temat kontroli FX.

Skopiuj kod do czystego bufora i posłuchaj. Nie zatrzymuj się jeszcze na tym etapie - rozpocznij zabawę z kodem. Zmień czas suwaków, nuty, syntezator, FX oraz czas uśpienia. Zauważysz, że możesz go przekształcić w coś zupełnie innego!

```
live_loop :moon_rise do
  with_fx :echo, mix: 0, mix_slide: 8 do |fx|
    control fx, mix: 1
    notes = (scale :e3, :minor_pentatonic, num_octaves: 2).shuffle
    sn = synth :prophet , sustain: 8, note: :e1, cutoff: 70, cutoff_slide: 8
    control sn, cutoff: 130
    sleep 2
    32.times do
      control sn, note: notes.tick, pan: rrand(-1, 1)
      sleep 0.125
    end
  end
end
```
