A.14 Modulacja Amplitudy

# Modulacja Amplitudy

W tym miesiącu zgłębimy jeden z najbardziej potężnych i elastycznych efektów audio dostępnych w Sonic Pi - efekt `:slicer`. Po przeczytaniu tego artykułu nauczysz się, w jaki sposób manipulować całkowitą głośnością wszystkich części naszego dźwięku kodowanego na żywo w nowe, bardzo potężne sposoby. Pozwoli Ci to na tworzenie nowych struktur rytmicznych i dźwiękowych oraz rozszerzy Twoje możliwości dźwiękowe.

## Pokrój tę Amplitudę

Ale co tak właściwie robi efekt `:slicer`? Jednym ze sposobów na myślenie o nim jest próba wyobrażenia sobie, że ktoś bawi się w kółko poziomem głośności na Twoim TV lub domowym zestawie hi-fi. Za chwilę się temu przyjrzymy, najpierw jednak posłuchajmy niskiego ryku poniższego kodu, który uruchamia syntezator `:prophet`:

```
synth :prophet, note: :e1, release: 8, cutoff: 70
synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
```

A teraz spróbujmy nałożyć na niego efekt `:slicer`:

```

with_fx :slicer do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

Posłuchaj, w jaki sposób efekt slicer przycisza i pogłaśnia dźwięk regularnie w rytm beatu. Zauważ również, jak efekt `:slicer` wpływa na cały dźwięk, który jest generowany spomiędzy bloku `do`/`end`. Możesz kontrolować prędkość, z jaką powoduje on włączanie i wyłączanie dźwięku za pomocą opcji fazy `phase:`, która kontroluje czas trwania pojedynczej fazy. Domyślnie jest to wartość `0.25`, co oznacza, że będzie to miało miejsce 4 razy na sekundę przy domyślnym tempie 60 BPM (ang. Beats Per Minute - ilość uderzeń na minutę). Spróbujmy to przyspieszyć:

```
with_fx :slicer, phase: 0.125 do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

A teraz spróbuj samodzielnie użyć innej długości fazy `phase:`. Spróbuj użyć dłuższych i krótszych wartości. Zobacz, co się stanie, jeśli wybierzesz naprawdę małą liczbę. Spróbuj również użyć innych syntezatorów, np. `:beep` lub `:dsaw` oraz innych nut. Zerknij na poniższy diagram, aby zobaczyć, jak różne wartości fazy `phase:` zmieniają ilość modulacji amplitudy przypadających na jedno uderzenie (beat).

![Phase Durations](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_phase_durations.png)

Czas trwania fazy to długość czasu, jaka jest potrzebna dla jednego włączenia/wyłączenia cyklu. Dlatego też mniejsze wartości spowodują, że efekt FX będzie włączany i wyłączany dużo częściej niż przy ustawieniu większych wartości. Dobrymi wartościami na początek zabawy są `0.125`, `0.25`, `0.5` i `1`.


## Kontrola Fal

Domyślnym zachowaniem efektu `:slicer` jest korzystanie z kwadratowej fali do manipulacji amplitudą na przestrzeni czasu. To właśnie dlatego słyszymy pełną amplitudę naszego dźwięku przez pewien moment, a potem nagle zostaje on całkowicie wyłączony na kolejny moment, potem znowu gra - i tak w kółko. Okazuje się jednak, że fala kwadratowa jest zaledwie jedną z 4 różnych fal, które mogą być używane przez efekt `:slicer`. Inne fale, z których możemy korzystać, to piła (ang. saw), trójkąt (ang. triangle) i sinusoida. Zerknij na poniższy diagram, aby zobaczyć, jak one wyglądają. Możemy również posłuchać, jak brzmią. Na przykład poniższy kod używa sinusoidy jako fali sterującej. Posłuchaj, jak dźwięk nie jest już włączany i wyłączany nagle, lecz zamiast tego płynnie pojawia się i zanika:

```
with_fx :slicer, phase: 0.5, wave: 3 do
  synth :dsaw, note: :e3, release: 8, cutoff: 120
  synth :dsaw, note: :e2, release: 8, cutoff: 100
end
```

Spróbuj pobawić się z różnymi formami fali poprzez zmianę opcji `wave:` na wartość `0`, aby uzyskać falę podobną do piły, `1` dla uzyskania fali o kwadratowym kształcie, `2` dla stworzenia fali trójkątnej oraz `3`, aby fala miała kształt sinusoidalny. Zobacz też, jak różne fale brzmią z innymi opcjami, na przykład `phase:`.

Każda z tych fal może zostać odwrócona za pomocą opcji `invert_wave:`, która obraca ją wokół osi y. Przykładowo w jednej fazie fala o kształcie piły zazwyczaj zaczyna się wysoko, powoli opada, po czym ponownie wskakuje na szczyt. Ustawiając opcję `invert_wave: 1`, fala taka zacznie się nisko i najpierw będzie powoli szła w górę do momentu, w którym z powrotem zeskoczy na dół. Oprócz tego możemy kontrolować falę poprzez uruchomienie jej w różnych momentach, korzystając z polecenia `phase_offset:` - używamy do tego wartości pomiędzy `0` a `1`. Bawiąc się i zmieniając wartości opcji `phase:`, `wave:`, `invert_wave:` i `phase_offset`, możesz znacząco wpłynąć na to, w jaki sposób amplituda jest zmieniana na przestrzeni czasu.

![Phase Durations](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_control_waves.png)


## Ustawianie własnych poziomów

Domyślnie `:slicer` zmienia się w zakresie amplitudy o wartościach od `1` (pełna głośność) do `0` (cisza). Może to zostać zmienione za pomocą opcji `amp_min:` (amplituda minimalna) oraz `amp_max:` (amplituda maksymalna). Możesz z nich skorzystać w połączeniu z falą sinusoidalną, aby stworzyć prosty efekt tremolo:

```
with_fx :slicer, amp_min: 0.25, amp_max: 0.75, wave: 3, phase: 0.25 do
  synth :saw, release: 8
end
```

To jest zupełnie tak jak złapanie pokrętła od głośności znajdującego się na Twoim sprzęcie hi-fi i przekręcanie go odrobinę w górę i w dół w taki sposób, że dźwięk 'drga' (z ang. wobble) w jedną i drugą stronę.


## Prawdopodobieństwa

Jednym z najbardziej potężnych właściwości efektu `:slicer` jest zdolność do używania prawdopodobieństwa, aby decydowało o tym, kiedy go włączać, a kiedy wyłączać. Zanim `:slicer` zacznie nową fazę, następuje rzut kostką i w zależności od wyniku albo zostaje użyta określona fala, albo dźwięk pozostanie wyciszony. Posłuchaj:

```
with_fx :slicer, phase: 0.125, probability: 0.6  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

Posłuchaj teraz, jak udało nam się uzyskać bardzo interesujący rytm impulsów. Spróbuj zmienić opcję `probability:` na inną wartość pomiędzy `0` a `1`. Te bliższe zeru spowodują, iż między każdym z dźwięków pojawi się więcej przestrzeni. Będzie to spowodowanie tym, że prawdopodobieństwo tego, iż dźwięki zostaną uruchomione, będzie znacznie większe.

Inną rzeczą godną zanotowania jest to, że system prawdopodobieństwa w efektach (FX) jest dokładnie taki sam jak system randomizacji (losowości), do którego można uzyskać dostęp przez funkcje takie jak `rand` czy `shuffle`. Oba są całkowicie przewidywalne. Oznacza to, że za każdym razem, gdy naciśniesz przycisk Run, usłyszysz dokładnie ten sam rytm pulsujący dla danego prawdopodobieństwa. Jeśli chciałbyś to zmienić, to zawsze możesz użyć opcji `seed:`, aby wybrać inny punkt początkowy dla losowych wartości. Działa to dokładnie tak samo jak funkcja `use_random_seed`, ale wpływa tylko na ten konkretny efekt.

Na koniec możesz zmienić pozycję `spoczynku` dla fali kontrolującej, kiedy test prawdopodobieństwa nie udaje się z wartości `0` na dowolną inną pozycję, wykorzystując opcję `prob_pos`:

```
with_fx :slicer, phase: 0.125, probability: 0.6, prob_pos: 1  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

## Przycinanie Beat'ów

Jedną naprawdę fajną rzeczą jest spróbowanie użycia efektu `:slicer` do pocięcia beatu perkusyjnego:

```
with_fx :slicer, phase: 0.125 do
  sample :loop_mika
end
```

Pozwala nam to na wzięcie dowolnego sampla i stworzenie zupełnie nowego brzmienia rytmicznego, co daje ogromną frajdę. Jednakże jedną rzeczą, o której należy pamiętać, jest to, aby upewnić się, iż tempo sampla pasuje do naszego aktualnego tempa BPM w Sonic Pi. W przeciwnym wypadku nasze cięcia będą brzmieć całkowicie beznadziejnie. Na przykład spróbuj zamienić sampel `:loop_mika` na `loop_amen`, aby usłyszeć, jak kiepsko może to brzmieć, kiedy tempo jest niewłaściwe.

## Zmiana tempa

Jak już widzieliśmy, zmiana domyślnego tempa BPM z wykorzystaniem polecenia `use_bpm` spowoduje, że wszystkie czasy odpoczynku (ang. sleep) oraz długość trwania obwiedni dźwięku syntezatorów zwiększą się lub skrócą, aby dopasować się do beatu. Dotyczy to również efektu `:slicer`, ponieważ długość trwania opcji `phase:` jest mierzona w beatach, a nie sekundach. Możemy zatem naprawić problem, jaki mamy w pętli `loop_amen` powyżej poprzez zmianę tempa BPM, tak aby pasowało do naszego sampla:

```
use_sample_bpm :loop_amen
with_fx :slicer, phase: 0.125 do
  sample :loop_amen
end
```

## Połączenie tego wszystkiego w jedną całość

Spróbujmy zastosować wszystkie te pomysły w finalnym przykładzie, który używa tylko samego efektu `:slicer` do stworzenia ciekawej kombinacji. Dalej, śmiało - zacznij zmieniać ten kawałek i zrób z niego swój nowy numer!

```
live_loop :dark_mist do
  co = (line 70, 130, steps: 8).tick
  with_fx :slicer, probability: 0.7, prob_pos: 1 do
    synth :prophet, note: :e1, release: 8, cutoff: co
  end
  
  with_fx :slicer, phase: [0.125, 0.25].choose do
    sample :guit_em9, rate: 0.5
  end
  sleep 8
end
live_loop :crashing_waves do
  with_fx :slicer, wave: 0, phase: 0.25 do
    sample :loop_mika, rate: 0.5
  end
  sleep 16
end
```




