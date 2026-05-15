10 Time-State (Stand der Zeit)

# Time-State (Stand der Zeit)

Oftmals ist es sinnvoll, Informationen zu haben, die *über verschiedenen Threads or Live-Loops geteilt werden kann*. Beispielsweise möchtest du vielleicht die aktuelle Tonart, BPM oder auch abstraktere Konzepte wie z. B. die aktuelle 'Komplexität' (die wiederum in den verschiedenen Threads unterschiedlich interpretiert werden könnte) teilen. Wenn wir dies tun, möchten wir aber auch nicht auf die garantierte Bestimmbarkeit von Sonic Pi verzichten. In anderen Worten: Wir möchten immer noch Programm-Code mit anderen teilen und sicherstellen können, dass wir genau wissen, was sie hören, wenn sie ihn ausführen. Am Ende des Abschnitts 5.6. dieses Tutorials haben wir kurz darüber gesprochen, warum wir *Variablen nicht nutzen sollten, um Informationen zwischen Threads zu teilen*, da wir dadurch diese Vorhersagbarkeit verlieren (aufgrund von Race Conditions).

Sonic Pi's Lösung für das Problem auf einfache Art mit globalen Variablen in einer vorhersagbaren Weise zu arbeiten, ist ein neuartiges System, das es Time-State (Stand der Zeit) nennt. Es mag erstmal komplex und schwierig klingen (Programmieren mit mehrfachen Threads und geteiltem Speicher werden normalerweise erst in der Universität behandelt). Allerdings wie du sehen wirst, genau wie beim Spielen deiner ersten Note, *macht es dir Sonic Pi unglaublich einfach, einen Zustand über mehrere Threads zu teilen* und dabei dennoch deine Programme *thread-safe und vorhersagbar" bleiben zu lassen.

Sag Hallo zu`get` und `set`...
