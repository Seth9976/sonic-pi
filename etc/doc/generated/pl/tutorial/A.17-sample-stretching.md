A.17 Rozciąganie Sampli

# Rozciąganie Sampli

Kiedy ludzie odkrywają Sonic Pi, jedną z pierwszych rzeczy, których się uczą, jest to, jak łatwo można z jego pomocą odtwarzać nagrane dźwięki, korzystając z funkcji `sample`. Na przykład możesz zagrać w pętli industrialny bęben, usłyszeć dźwięk chóru, a nawet dźwięk scratch'ującego winyla za pomocą pojedynczej linii kodu. Niezależnie od tego, wiele z tych osób nie zdaje sobie sprawy z tego, że można zmieniać prędkość, z jaką jest odtwarzany dany sampel, aby uzyskać różne bardzo dobre efekty i równocześnie zupełnie nowy poziom kontroli nad swoimi nagranymi dźwiękami. Nie czekaj więc, tylko odpal swoją kopię Sonic Pi i porozciągajmy jakieś sample!

## Spowalnianie Sampli

Aby zmienić tempo odtwarzania sampla, musimy użyć opcji `rate:`:

```
sample :guit_em9, rate: 1
```
    
Jeśli ustawimy wartość opcji `rate:` jako `1`, to sampel zostanie zagrany z normalną szybkością. Jeśli chcemy zagrać go ponownie z prędkością zmniejszoną o połowę, wystarczy, że dla opcji `rate:` użyjemy wartości `0.5`:


```
sample :guit_em9, rate: 0.5
```
    
Zauważ, że odciśnie to na nasz dźwięk dwa efekty. Po pierwsze sampel będzie miał niższą wysokość dźwięku, po drugie zagranie go zajmie dwa razy tyle czasu co normalnie (zerknij na pasek boczny, aby uzyskać wyjaśnienie, dlaczego tak się dzieje). Możemy nawet wybierać coraz mniejsze i mniejsze wartości, zbliżając się do `0`, i tak szybkość `rate:` o wartości `0.25` będzie jedną czwartą prędkości, `0.1` będzie jedną dziesiątą prędkości itd. Spróbuj pobawić się z różnymi niskimi wartościami opcji `rate:` i zobacz, czy uda Ci się uzyskać bardzo niskie dudnienie.

## Przyśpieszanie Sampli

Oprócz możliwości sprawiania, aby dźwięk był dłuższy oraz niższy za pomocą niskich wartości tempa, możemy użyć wyższego tempa w celu skrócenia dźwięku i sprawienia, by był wyższy. Tym razem spróbujmy wziąć na warsztat pętlę z bębnem. Najpierw posłuchaj, jak brzmi ten dźwięk na domyślnym tempie równym `1`:

```
sample :loop_amen, rate: 1
```


A teraz spróbujmy odrobinę go przyspieszyć:

```
sample :loop_amen, rate: 1.5
```
    
Ha! Właśnie przeszliśmy na inny gatunek muzyczny - z oldschool'owego techno do jungle. Zauważ, że zmieniła się zarówno wysokość każdego uderzenia bębna, jak i cały rytm uległ przyśpieszeniu. Teraz spróbuj wykorzystać jeszcze wyższe tempo i zobacz, jak wysoką i jak krótką pętlę z bębnem uda Ci się stworzyć. Na przykład, jeśli użyjesz tempa o wartości `100`, pętla zmieni się w kliknięcie!

## Wsteczny Bieg

Teraz jestem pewien, że w tej chwili wielu z Was ma w głowie tę samą rzecz... "A co, jeśli użyjesz minusowej liczby dla tempa?". Ciekawe pytanie! Pomyślmy zatem o tym przez chwilkę. Jeśli nasza wartość `rate:` oznacza prędkość, z którą odtwarzany jest dany sampel, czyli `1` - normalna szybkość, pod `2` kryje się podwójna, `0.5` oznacza tempo mniejsze o połowę, to wartość `-1` spowoduje, iż dźwięk zostanie zagrany od końca! Wypróbujmy to na "snare", lecz rozpoczynając od normalnego tempa:

```
sample :elec_filt_snare, rate: 1
```
    
Świetnie! Wybrany kawałek jest odtwarzany od końca!

```
sample :elec_filt_snare, rate: -1
```
    
Oczywiście możesz zagrać go od końca dwa razy szybciej z wartością `-2` albo dwa razy wolniej za pomocą `-0.5`. Obeznaj się z różnymi minusowymi tempami oraz baw się dobrze! Ten efekt brzmi całkiem zabawnie z samplem `:misc_burp`!


## Próbki, Tempo i Wysokość [Panel Boczny]

Jednym z efektów zmiany tempa sampli jest to, iż szybsze wartości oznaczają wyższą wysokość dźwięku i na odwrót - wolniejsze zaś niższą. Słyszysz go prawdopodobnie codziennie w swoim życiu - wtedy, gdy jeździsz na rowerze lub przechodzisz obok służb na sygnale. Im bliżej dźwięku, tym wysokość jest wyższa, im dalej zaś, tym jest niższa. Nazywa się to efektem Dopplera. Dlaczego tak się dzieje?

Rozważmy prosty sygnał dźwiękowy reprezentowany przez falę sinusoidalną. Jeżeli użyjemy oscyloskopu, by rozrysować dźwięk, zobaczymy coś podobnego do Figury A. Dźwięk z wyższą oktawą reprezentuje Figura B, a z niższą - Figura C. Zauważ to, iż fale wyższych nut są bardziej ściśle oraz niższych - bardziej rozłożone.

Sampel sygnału jest niczym innym jak wiele liczb (x, y, współrzędne), które rozrysowane na grafie odrysowują pierwotne krzywe. Spójrz na figurę D, gdzie każde koło reprezentuje współrzędne. By zamienić je ponownie w dźwięk, komputer pracuje przy każdej wartości x i wysyła odpowiednie wartości y do głośników. Magią jest to, iż tempo, nad którym pracuje maszyna poprzez numery x, nie musi być takie same jak tempo, z którym zostały nagrane. Używając innych słów - przestrzeń (reprezentująca ilość czasu) między każdym kołem może zostać rozciągnięta lub ściśnięta. Więc jeśli komputer przechodzi przez wartość x szybciej niż za pomocą oryginalnego tempa, wpłynie to na efekt zgniatania kół ze sobą, czego rezultatem będzie wyżej brzmiący sygnał. To także uczyni go krótszym, poprzez szybszą pracę przez koła. Zostało to pokazane na Figurze E.

Wreszcie ostatnią rzeczą potrzebną do zrozumienia tego wszystkiego jest matematyk o imieniu Fourier, który udowodnił, że każdy dźwięk to tak naprawdę wiele fal sinusoidalnych połączonych w jedną całość. W związku z tym ściśnięcie i rozciągnięcie każdego nagranego sygnału oznacza wykonanie tej czynności na wielu falach sinusoidalnych - w tym samym czasie i w tej samej formie.

## Funkcja Pitch Bending

Jak widzimy, używanie szybszego tempa powoduje, iż dźwięk będzie miał wyższą wysokość i na odwrót - niższe tempo = niższa wysokość. Bardzo prosta i użyteczna sztuczka dotyczy tego, iż wysokość większa o oktawę jest rezultatem podwójnego tempa i na odwrót - wysokość mniejsza o oktawę jest rezultatem dwa razy mniejszego tempa. Melodyjne sample grane jeden obok drugiego brzmią całkiem fajnie:

```
sample :bass_trance_c, rate: 1
sample :bass_trance_c, rate: 2
sample :bass_trance_c, rate: 0.5
```
    
Jednakże co w przypadku, gdy po prostu chcemy zmienić tempo tak, aby wysokość zwiększyła się o jeden półton (jedną nutę na pianinie)? Sonic Pi sprawia, iż jest to banalnie proste - wystarczy użyć parametru `rpitch:`:

```
sample :bass_trance_c
sample :bass_trance_c, rpitch: 3
sample :bass_trance_c, rpitch: 7
```
    
Jeśli spojrzysz na logi po prawej stronie, to zauważysz, iż `rpitch:` trójki aktualnie odpowiada tempie o wartości `1.1892` i `rpitch:` siódemki odpowiada tempie o wartości `1.4983`. Wreszcie możemy także łączyć ze sobą parametry `rate:` oraz `rpitch:`:

```
sample :ambi_choir, rate: 0.25, rpitch: 3
sleep 3
sample :ambi_choir, rate: 0.25, rpitch: 5
sleep 2
sample :ambi_choir, rate: 0.25, rpitch: 6
sleep 1
sample :ambi_choir, rate: 0.25, rpitch: 1
```
    

## Połączenie tego wszystkiego w jedną całość

Spójrzmy na zwyczajny kawałek, który łączy te pomysły. Skopiuj go do wolnego bufora Sonic Pi, naciśnij play, wsłuchaj się przez chwilę oraz użyj jako punktu startowego dla Twojego własnego kodu. Zobacz, jak zabawna jest kontrola tempa odtwarzanych sampli. Jako ćwiczenie spróbuj nagrać Twoje własne dźwięki i pobaw się z wartością parametru `rate:`, aby zobaczyć, jak szalone kombinacje możesz stworzyć.

```
live_loop :beats do
  sample :guit_em9, rate: [0.25, 0.5, -1].choose, amp: 2
  sample :loop_garzul, rate: [0.5, 1].choose
  sleep 8
end
 
live_loop :melody do
  oct = [-1, 1, 2].choose * 12
  with_fx :reverb, amp: 2 do
    16.times do
      n = (scale 0, :minor_pentatonic).choose
      sample :bass_voxy_hit_c, rpitch: n + 4 + oct
      sleep 0.125
    end
  end
end
```







