10 Stan Czasu

# Stan czasu

Bardzo często przydatne jest posiadanie informacji, która jest *współdzielona pomiędzy wieloma wątkami lub żywymi pętlami*. Na przykład, chciałbyś współdzielić aktualną gamę, BPM albo może nawet bardziej abstrakcyjne koncepty takie jak aktualna 'złożoność' (coś co mógłbyś interpretować na różne sposoby w różnych wątkach). Ponadto nie chcemy także zgubić żadnego z naszych istniejących i deterministycznych mechanizmów gdy to robimy. Innymi słowy, chcielibyśmy być w stanie współdzielić kod z innymi i być stuprocentowo pewnymi tego, że usłyszą to co chcemy gdy go uruchomią. Pod koniec Sekcji 5.6 tego samouczka krótko wyjaśniliśmy dlaczego *nie powinniśmy używać zmiennych do współdzielenia informacji pomiędzy różnymi wątkami* ze względu na możliwość utraty determinizmu (to z kolei z powodu wyścigu wątków).

Sonic Pi umożliwia rozwiązanie problemu łatwej pracy z globalnymi zmiennymi w deterministyczny sposób poprzez nowatorski system, który nazywa się Stan Czasu. Może to brzmieć trochę skomplikowanie (i rzeczywiście, w Wielkiej Brytanii, programowanie z wykorzystaniem wielu wątków i współdzielonej pamięci to temat na poziomie typowo uniwersyteckim). Jednakże, jak niedługo zobaczysz, dokładnie tak samo jak zagranie twojej pierwszej nuty, *Sonic Pi sprawia, że współdzielenie stanu pomiędzy wątkami jest niezwykle łatwe* a jednocześnie sprawia, że twoje programy są *bezpieczne z punktu widzenia wielowątkowości oraz deterministyczne.*.

Poznaj `get` i `set`...
