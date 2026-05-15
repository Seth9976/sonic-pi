10 Time State

# Time State

È spesso utile avere informazioni che *si condividono attraverso vari thread o cicli vivi*. Per esempio, forse vuoi condividere l'idea della chiave attuale, BPM, o anche concetti più astratti come la "complessità" attuale (che si potrebbe interpretare diversamente in diversi thread). Non vogliamo nemmeno perdere le garanzie deterministiche quando facciamo questo. Cioè, vorremmo condividere il nostro codice con gli altri e sapere esattamente ciò che sentiranno quando lo eseguono. Alla fine della Sezione 5.6 di questa lezione, abbiamo trattato in breve la ragione perché *non dobbiamo usare variabili per condividere informazioni attraverso i thread*, dovuta alla perdita del determinismo (a sua volta dovuta alle condizioni di gara).

La soluzione di Sonic Pi al problema di lavorare facilmente con variabili globali in un modo deterministico è con un sistema nuovo chiamato Time State (stato temporale). Può sembrare complesso e difficile (infatti, nel Regno Unito, programmazione con thread molteplici e memoria condivisa è tipicamente una matteria al livello universitario). Ma, come vedrai, simile a suonare la prima nota, *Sonic Pi facilita molto la condivisione di stato attraverso vari thread* mentre mantiene i tuoi programmi thread-safe e deterministici*.

Ti presento `get` e `set`...
