A.12 Krojenie Sampli

# Krojenie Sampli

Jakiś czas temu w 3 odcinku serii poradników Sonic Pi dowiedzieliśmy się, w jaki sposób zapętlać, rozciągać i filtrować jeden z najbardziej popularnych sampli drum break'owych w historii - Amen Break. W tym numerze wykonamy kolejny krok i nauczymy się, w jaki sposób możemy go pociąć na kawałki, przetasować je i na koniec skleić ponownie w zupełnie nowy sposób. Nie przejmuj się, jeśli brzmi to dla Ciebie odrobinę zwariowanie. Za chwilę wszystko stanie się jasne i niedługo opanujesz zupełnie nowe narzędzie, którego będziesz używał w swoich kodowanych na żywo setach.

## Dźwięk jako Dane

Zanim jednak zaczniemy, poświęćmy krótką chwilę, aby zrozumieć, w jaki sposób pracuje się z samplami. Podejrzewam, że każdy z Was miał już do czynienia z potężnym samplerem Sonic Pi. Jeśli nie - jest to najlepszy moment ku temu! Włącz swojego Raspberry Pi, uruchom Sonic Pi, wybierając go z Menu zawierającego aplikacje związane z programowaniem, wprowadź następujący kawałek kodu do czystego buforu, a następnie naciśnij przycisk Run - usłyszysz nagrany wcześniej beat z perkusją:

```
sample :loop_amen
```

Nagranie dźwięku jest prosto reprezentowane jako dane - dużo liczb pomiędzy -1 a 1, które odzwierciedlają szczyty i doliny danej fali dźwięku. Jeśli zagramy te liczby w odwrotnej kolejności, otrzymamy oryginalne brzmienie. Jednakże nic nas nie powstrzymuje przed tym, żeby zagrać je odwrotnie i stworzyć nowe brzmienie, prawda?

Ale w jaki sposób tak naprawdę sample są nagrywane? W sumie jest to dość proste, gdy już zrozumiesz podstawowe prawa, jakimi rządzi się dźwięk. Kiedy wydajesz jakiś dźwięk, na przykład poprzez uderzenie bębna, hałas przemieszcza się przez powietrze w sposób bardzo podobny tego, w jaki faluje tafla jeziora, kiedy wrzucisz do niego kamyk. Kiedy te fale dotrą do Twoich uszu, Twój bębenek porusza się sympatycznie i przetwarza te ruchy na dźwięk, który słyszysz. Jeśli chcemy nagrać i odtworzyć ten dźwięk ponownie, potrzebny jest nam sposób, w który moglibyśmy uchwycić, zapisać i odtworzyć te fale. Jednym ze sposobów jest wykorzystanie mikrofonu, który zachowuje się tak jak bębenek oraz porusza się tam i z powrotem, gdy kolejne fale dźwięku uderzają w niego. Następnie mikrofon konwertuje jego pozycję na małe elektroniczne sygnały, które są następnie mierzone wiele razy na sekundę. Te pomiary są następnie odwzorowane jako seria liczb pomiędzy -1 i 1.

Jeśli chcielibyśmy narysować wykres wizualizujący dźwięk, byłby to prosty graf przedstawiający dane, gdzie x byłoby osią czasu, natomiast aktualna pozycja mikrofonu/głośnika byłaby oznaczona na osi y jako wartość pomiędzy -1 a 1. Możesz zobaczyć przykład takiego grafu na samej górze diagramu.

## Granie Wycinka Sampla

Zastanówmy się zatem, w jaki sposób możemy zakodować Sonic Pi, aby zagrało dany sampel od tyłu? Żeby odpowiedzieć na te pytanie, musimy przyjrzeć się opcjom `start:` i `finish:`, które są zdefiniowane dla polecenia `sample`. Pozwalają nam one na kontrolę momentu początku i zakończenia naszego dźwięku za pomocą liczb reprezentujących dźwięk. Wartości te dla obu opcji są reprezentowane jako liczby pomiędzy `0` i `1`, gdzie `0` stanowi początek danego sampla, natomiast `1` jest jego końcem. Aby więc zagrać pierwszą połowę sampla Amen Break, wystarczy, że w opcji `finish:` ustawimy wartość `0.5`:

```
sample :loop_amen, finish: 0.5
```

Możemy ponadto zdefiniować wartość początku opcją `start:` dla zagrania jeszcze mniejszego wycinka sampla:

```
sample :loop_amen, start: 0.25, finish: 0.5
```

Dla zabawy możesz nawet ustawić opcję `finish:`, tak aby była *przed* opcją `start:`, a spowodujesz, że ta część sampla zostanie zagrana od tyłu:

```
sample :loop_amen, start: 0.5, finish: 0.25
```

## Zmiana kolejności Odtwarzania Sampla

Teraz, gdy już wiemy, że sampel to nic innego jak tylko lista liczb, które mogą być zagrane w odwróconej kolejności oraz w jaki sposób możemy zagrać określoną część danego sampla, możemy zacząć zabawę w zagranie sampla od tyłu w 'niewłaściwej` kolejności.

![Amen Slices](../../../etc/doc/images/tutorial/articles/A.12-sample-slicing/amen_slice.png)

Spróbujmy wziąć nasz sampel Amen Break, pociąć go na 8 równych kawałków, a następnie przetasować te części. Spójrz na diagram: na samej górze sekcja A) reprezentuje diagram przedstawiający dane naszego oryginalnego sampla. Pocięcie go na 8 kawałków da nam to, co widzimy w sekcji B) - zauważ, że nadaliśmy każdej części inny kolor, abyśmy mogli łatwiej je rozróżnić. Możesz zauważyć, że każdy kawałek ma na górze wartości początku i końca. Na koniec w sekcji C) widzimy jedną z możliwych kombinacji przetasowania tych kawałków. Następnie możemy zagrać te części, aby stworzyć zupełnie nowy beat. Zobacz na poniższy kawałek kodu, który robi te rzeczy:

```
live_loop :beat_slicer do
  slice_idx = rand_i(8)
  slice_size = 0.125
  s = slice_idx * slice_size
  f = s + slice_size
  sample :loop_amen, start: s, finish: f
  sleep sample_duration :loop_amen, start: s, finish: f
end
```

1. wybieramy losowe cięcie do zagrania, co powinno być losową liczbą z zakresu od 0 do 7 (pamiętaj, że zaczynamy liczyć od 0). Sonic Pi posiada bardzo przydatną funkcję, która idealnie nadaje się do tego celu : `rand_i(8)`. Następnie zapisujemy indeks tego losowego wycinka w zmiennej `slice_idx`.
2. Definiujemy rozmiar dla każdego kawałka (`slice_size`), który w naszym przypadku ma wielkość 1/8 lub 0.125. Wielkość wycinka `slice_size` jest nam niezbędna, abyśmy mogli skonwertować nasz indeks `slice_idx` do wartości pomiędzy 0 a 1, tak żebyśmy użyli go jako nasza opcja startu `start:`.
3. Aby obliczyć pozycję startu `s`, mnożymy indeks `slice_idx` przez wielkość wycinka `slice_size`.
4. Obliczamy pozycję końcową `f` poprzez dodanie wielkości wycinka `slice_size` do pozycji początkowej `s`.
5. Teraz możemy zagrać wycinek sampla poprzez podłączenie wartości `s` i `f` po opcje `start:` i `finish:` polecenia `sample`.
6. Zanim zagramy kolejny kawałek, musimy wiedzieć, jak długo powinniśmy poczekać (`sleep`) - powinien to być czas równy długości trwania jednego wycinka sampla. Na szczęście Sonic Pi udostępnia nam funkcję `sample_duration`, akceptującą wszystkie te same opcje, które możemy przekazać do polecenia `sample` i po prostu zwraca długość trwania. W związku z tym poprzez przekazanie do polecenia `sample_duration` naszych opcji `start:` i `finish:`, możemy łatwo dowiedzieć się, jaki jest czas trwania pojedynczego wycinka sampla.
7. Cały ten kod opakowujemy w żywą pętlę `live_loop`, dzięki temu w kółko wybieramy kolejne losowe kawałki do zagrania.


## Połączenie tego wszystkiego w jedną całość

Spróbujmy teraz połączyć wszystko to, co do tej pory widzieliśmy i stwórzmy finałowy przykład, który zademonstruje, jak możemy wykorzystać podobne podejście do połączenia losowo pociętych beatów z odrobiną basu, aby utworzyć zaczyn ciekawego utworu. Teraz Twoja kolej - użyj poniższego kodu jako punkt wyjściowy i zobacz, czy potrafisz zmienić go po swojemu, aby utworzyć coś nowego...

```
live_loop :sliced_amen do
  n = 8
  s =  line(0, 1, steps: n).choose
  f = s + (1.0 / n)
  sample :loop_amen, beat_stretch: 2, start: s, finish: f
  sleep 2.0  / n
end
live_loop :acid_bass do
  with_fx :reverb, room: 1, reps: 32, amp: 0.6 do
    tick
    n = (octs :e0, 3).look - (knit 0, 3 * 8, -4, 3 * 8).look
    co = rrand(70, 110)
    synth :beep, note: n + 36, release: 0.1, wave: 0, cutoff: co
    synth :tb303, note: n, release: 0.2, wave: 0, cutoff: co
    sleep (ring 0.125, 0.25).look
  end
end
```
