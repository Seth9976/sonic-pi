A.2 Kodowanie na żywo

# Kodowanie na żywo

Promienie lasera przecinały smugi dymu, a bas pulsujący z subwoofera przeszywał ciała tłumu na parkiecie. Atmosfera była gorąca, tworzyły ją syntezatory i taniec. W klubie tym było jednak coś dziwnego. Na ścianie, tuż nad głową DJ'a, był wyświetlany jasny, futurystyczny tekst, który bez przerwy się poruszał, zmieniał, tańczył i świecił... Nie były to żadne fantazyjne wizualizacje, tylko Sonic Pi uruchomiony na Raspberry Pi. Osoba okupująca stanowisko DJ'a nie kręciła płytami winylowymi, lecz pisała, edytowała i uruchamiała kod. Na żywo...! To jest właśnie kodowanie na żywo.

![Sam Aaron kodujący na żywo](../../../etc/doc/images/tutorial/articles/A.02-live-coding/sam-aaron-live-coding.png)

Być może brzmi to jak historia z przyszłości opisująca futurystyczny klub, ale kodowanie muzyki w ten sposób to rosnący trend i często jest opisywany jako kodowanie na żywo (http://toplap.org). Jednym z obecnych kierunków, jaki obrał ruch tworzący muzykę w taki sposób, jest Algorave (http://algorave.com) - wydarzenia, podczas których osoby takie jak ja kodują muzykę do tańca dla ludzi. Jednakże, wcale nie musisz być w klubie, żeby Kodować Na Żywo z Sonic Pi v2.6+, możesz to robić gdziekolwiek tylko Ci się uda zabrać ze sobą swój komputer, parę słuchawek lub głośniki. Kiedy skończysz czytać ten artykuł, będziesz programował swoje własne bity i zmieniał je na żywo. Gdzie pójdziesz - dalej będzie ograniczone tylko Twoją wyobraźnią.

## Live Loop (Żywa Pętla)

Kluczem do kodowania na żywo jest okiełznanie polecenia `live_loop`. Przyjrzyjmy się jednej:

```
live_loop :beats do
  sample :bd_haus
  sleep 0.5
end
```

Aby stworzyć żywą pętlę (`live_loop`), potrzebujemy 4 składników. Pierwszym jest jej nazwa. Nasza pętla `live_loop` powyżej została nazwana `:beats`. Masz pełną dowolność w wyborze nazwy dla Twojej pętli ` live_loop`. Możesz tutaj szaleć do woli! Bądź kreatywny - często używam nazw, które mówią publiczności coś o tworzonej muzyce. Drugi składnik to słowo kluczowe `do` pojawiające się tam, gdzie zaczyna się dana pętla `live_loop`. Trzeci składnik to `end`, które zaznacza, gdzie nasza pętla `live_loop` się kończy. Ostatni składnik to ciało żywej pętli `live_loop` opisujące, co dana pętla będzie powtarzać - kawałek pomiędzy słowami kluczowymi `do` i `end`. W tym przypadku będziemy w koło odtwarzać sampel grający bęben i czekać przez połowę uderzenia. To spowoduje, że usłyszymy przyjemne i regularne uderzenia bębna. Śmiało, skopiuj ten kawałek do pustego buforu w Sonic Pi i naciśnij przycisk Run. Bum, Bum, Bum!.

## Redefiniowanie w locie

No dobra, ale co w takim razie jest takiego wyjątkowego w żywej pętli `live_loop`? Jak do tej pory wygląda to jak zwykła pętla `loop`, która została wyniesiona na piedestał. Całe piękno pętli `live_loop` polega na tym, że możemy zmieniać ją w locie. Oznacza to, iż możemy mamy możliwość zmiany jej zachowania. Jest to klucz do kodowania na żywo. Spróbujmy zrobić coś takiego:

```
live_loop :choral_drone do
  sample :ambi_choir, rate: 0.4
  sleep 1
end
```

A teraz naciśnij przycisk Run lub wciśnij klawisze `alt-r`. Słyszysz teraz parę wspaniałych dźwięków chóralnych. A teraz, kiedy pętla wciąż gra, zmień wartości opcji rate z `0.4` na `0.38`. Wciśnij Run ponownie. Łał! Zauważyłeś, że chór wybrzmiewa teraz inną nutą? Zmień ją ponownie na wartość `0.4`, aby przywrócić wcześniejsze brzmienie. Teraz obniż wartość na `0.2`, potem na `0.19` i wróć do `0.4`. Widzisz, jak zmiana tylko jednego parametru w locie daje Ci realną kontrolę nad muzyką? Spróbuj teraz pobawić się parametrem rate samodzielnie - wybierz swoje własne wartości. Spróbuj użyć liczb ujemnych, naprawdę bardzo małych wartości oraz dużych. Przyjemnej zabawy!

## Spanie (ang. sleep) jest ważne

Jedną z najważniejszych lekcji dotyczących pętli `live_loop` jest to, że potrzebują one odpoczynku. Przyjrzyjmy się następującej pętli `live_loop`:

```
live_loop :infinite_impossibilities do
  sample :ambi_choir
end
```

Jeśli spróbujesz uruchomić powyższy kawałek kodu, szybko zauważysz, że pętla `live_loop` zacznie narzekać, że nie spała. To jest miejsce, w którym do akcji wkracza system bezpieczeństwa! Zatrzymaj się na chwilę i zastanów się, czego ten kawałek kodu oczekuje od komputera. Tak jest, prosi komputer, żeby zagrał sampel ambi_hoir nieskończoną ilość razy od razu. Gdyby nie system bezpieczeństwa, to nasz biedny komputer spróbowałby zrobić to, o co go prosimy, w wyniku czego popsułby się i spalił. Pamiętaj więc, Twoje żywe pętle muszą posiadać polecenie `sleep`.


## Łączenie Dźwięków

Muzyka jest pełna rzeczy, które się dzieją w tym samym czasie. Bębny grają, a w tym samym czasie słychać bas, wokal i gitarę... W komputerach nazywamy to współbieżnością, a Sonic Pi pozwala nam na granie różnych dźwięków w tym samym czasie w niesamowicie prosty sposób. Wystarczy, że użyjemy więcej niż jednej pętli `live_loop`!

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

Mamy tu dwie pętle `live_loop` - jedna kręci się szybko, wygrywając rytm, natomiast druga kręci się powoli, tworząc zwariowaną sekcję basową.

Jedną z interesujących rzeczy w używaniu wielu pętli `live_loop` jest to, że każda z nich zarządza swoim czasem. Oznacza to, iż bardzo łatwo można stworzyć interesujące struktury polirytmiczne, a nawet grać fazami w stylu Stevena Reicha! Spróbuj tego:

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

## Połączenie tego wszystkiego w jedną całość

Każdy z tutoriali ma swoje zwieńczenie, którym jest przykład końcowy. Jest to kawałek muzyki reprezentujący wszystkie idee zaprezentowane w danej sekcji. Przeczytaj ten kod i zobacz, czy jesteś w stanie wyobrazić sobie, co ten kod robi. Następnie skopiuj go do czystego buforu w Sonic Pi i naciśnij przycisk Run, aby usłyszeć, jak brzmi w rzeczywistości. Na koniec zmień wybraną przez siebie liczbę albo wykomentuj lub odkomentuj wybrane według własnego uznania linijki kodu. Pomyśl, czy możesz użyć tego kawałka jako punkt wyjściowy dla nowego występu, a co najważniejsze - spróbuj się dobrze bawić. Do zobaczenia następnym razem...

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
