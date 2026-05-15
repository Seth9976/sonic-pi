10 Tijd toestand

# Tijd toestand

Vaak is het nuttig om informatie te *delen over meerder threads of live lussen*. Je zou bijvoorbeeld de huidige toonsoort, BPM of meer abstracte concepten zoals de huidige 'complexiteit' (wat je potentieel op verschillende manieren interpreteerd in verschillende threads). We willen ook niet iets van ons bestaande garanties voor determinisme verliezen wanneer we dit doen. In andere woorden, we willen nog steeds code met andere kunnen delen en exact weten wat ze horen wanneer het wordt uitgevoerd. Aan het einde van sectie 5.6 van deze tutorial wordt kort besproken waarom we *geen variabelen zouden moeten gebruiken om informatie te delen tussen threads* vanwege het verlies van determinisme (op hun beurt veroorzaakt door race condities).

Sonic Pi's oplossing voor het probleem van "makkelijk werken met globale variabelen op een deterministische wijze" is door een vernieuwend systeem dat "Time State" heet. Dit kan ingewikkeld en moeilijk klinken (in feite is programmeren met meerdere threads en een gedeeld geheugenmodel typisch een onderwerp op universitair niveau). Echter, zoals je zult zien, is het net als het spelen van de eerste noot. *Sonic Pi maakt het ontzettend eenvoudig om toestand te delen tussen threads* terwijl je programma's nog steeds *thread safe en deterministisch blijven*.

Maak kennis met `get` en `set`...
