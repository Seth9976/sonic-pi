11 MIDI

# MIDI

Wanneer je het converteren van code naar muziek volledig beheerst, zou je jezelf kunnen afvragen: "Wat is het volgende?". Soms kan het uitsluitend werken binnen de syntax van Sonic Pi en het geluidssysteem uitdagend genoeg zijn om je in een nieuwe creatieve positie te stoppen. Het is echter soms noodzakelijk om uit de code naar de echte wereld te stappen. We willen twee extra dingen:

1. Om acties vanuit de echte wereld te converteren naar Sonic Pi events om mee te coderen
2. Om vanuit Sonic Pi's sterke timing model en semantiek objecten in de echte wereld te controleren en manipuleren

Gelukkig bestaat er sinds de jaren 80 een protocol wat precies deze interactie levert - MIDI. Er is een groot aantal externe apparaten beschikbaar, waaronder keyboards, controllers en sequencers, naast diverse pro audio software die allemaal MIDI ondersteunen. We kunnen MIDI gebruiken om data op te halen en te versturen.

Sonic Pi levert volledige ondersteuning voor het MIDI protocol waarmee je jouw live code kunt verbinden aan de echte wereld. Laten we hier verder induiken.
