11 MIDI

# MIDI

さて、コードを音楽に変換することをマスターしたあなたはこう思うかもしれません—次は何をすればいいんだろう？ Sonic Piの構文やサウンドシステムの中で作業するという制約は時に刺激的で、新しい創造性をもたらすこともあるでしょう。しかし、コードを現実世界に持ち出すことも時には必要なのです。私たちが欲しいのは次の2つの追加要素です：

1. 現実世界での行動をSonic Piのイベントに変換し、コーディングできるようになること
2. To be able to use Sonic Pi's strong timing model and semantics to control and manipulate objects in the real world

Luckily there's a protocol that's been around since the 80s that enables exactly this kind of interaction - MIDI. There's an incredible number of external devices including keyboards, controllers, sequencers, and pro audio software that all support MIDI. We can use MIDI to receive data and also use it to send data.

Sonic Pi provides full support for the MIDI protocol enabling you to connect your live code to the real world. Let's explore it further...
