A.2 ライブコーディング

# ライブコーディング

漂うスモークをレーザー光線がスライスし、サブウーファーは観衆の体の奥深くまで低音を震わせている。その雰囲気は、眩暈のするようなシンセのミックスとダンスで満たされている。しかし、このクラブでは何かがおかしい。DJブースの上に映し出された明るい色は、小刻みに動き、舞い、きらめく未来的なテキストである。それは、手の込んだビジュアルではなく、Raspberry Pi上で動いているSonic Piを単に映し出したものである。DJブースでは、レコードを回しているのではなく、コードを書き、編集し、実行しているのである。ライブである。これがライブコーディングである。

![Sam Aaron Live Coding](../../../etc/doc/images/tutorial/articles/A.02-live-coding/sam-aaron-live-coding.png)

これは未来のクラブの現実的でない話のように聞こえるかもしれませんが、音楽をコーディングすることは盛り上がりつつあるトレンドであり、しばしばライブコーディング（http://toplap.org）と呼ばれています。この音楽制作アプローチの最近の流れの1つは、Algorave（http://algorave.com）です。私のようなアーティストが、人々をダンスさせるために音楽をコーディングするイベントです。しかし、ライブコーディングするのにクラブに居る必要はありません。Sonic Pi v2.6以上では、ヘッドホンやスピーカーとRaspberry Piを持っていける場所であればどこでも、ライブコーディングができます。一度この記事を最後まで読めば、あなた自身のビートをプログラミングでき、それをライブで変更できるでしょう。その後どこに進むかは、あなたのイマジネーションにだけ制約されるでしょう。

## ライブループ

Sonic Piでライブコーディングする鍵は、`live_loop`をマスターすることです。次のコードを見てみましょう。

```
live_loop :beats do
  sample :bd_haus
  sleep 0.5
end
```

`live_loop`には、4つのコアな構成要素があります。1つめは、その名前です。上の`live_loop`の名前は`:beats`です。`live_loop`はどんな名前でも自由に付けることができます。創造的に、またクレイジーになりましょう。私は、作っている音楽に関して何か観衆と意思疎通するような名前をしばしば付けます。2つめの構成要素は`do`という単語で、`live_loop`の開始する位置を示しています。3つめは`end`で`live_loop`を終了する位置を示しています。そして最後は、`live_loop`のボディ部分です。これは`do`と`end`に囲まれたコードで、ループで何を繰り返すかが記述されている箇所です。上のコードでは、定期的にバスドラムのサンプルを再生と半拍分の待機を繰り返しています。これにより、心地良い規則的なビートが作られています。さあ、Sonic PiのBufferに上のコードをコピーして実行してみましょう。Boom, Boom, Boom!

## その場で再定義する

では、`live_loop`の何が特別なのでしょう？ 見たところ`loop`にそっくりですね！ ええと、`live_loop`の利点は、その場で再定義できることです。これは、ライブループが実行されている間にも、その中でやっていることが変更できるということです。これがSonic Piのライブコーディングの秘訣です。試してみましょう。

```
live_loop :choral_drone do
  sample :ambi_choir, rate: 0.4
  sleep 1
end
```

では、`Run`ボタンをクリックするか、キーボードから`alt-r`を叩いてみてください。美しいコーラスのサウンドが聞こえると思います。次に、それが演奏されている間に、rateを`0.4`から`0.38`に変更して、`Run`を叩いてみてください。ワオ！ コーラスの音程が変わったのが聞こえましたか？ rateを`0.4`に上げて、値を戻すとどうなるか聞いてみましょう。続いて、rateを`0.2`に下げ、さらに`0.19`に下げ、再び`0.4`にあげてみましょう。たった1つのオプションをその場で変更するだけで、音楽をコントロールできるのを実感できるでしょう。では、rateを自分自身で選んで遊んでみましょう。マイナスの数、とても小さい数や大きい数を試してみましょう。楽しんでみてください！

## 休みは大切

`live_loop`に関する最も重要な教訓の1つは、それを*休ませる*ことが必要だということです。次の`live_loop`を考えてみましょう。

```
live_loop :infinite_impossibilities do
  sample :ambi_choir
end
```

このコードを実行しようとすると、`live_loop`が`sleep`していないとSonic Piがエラーを出すでしょう。これはセーフティシステムが実行されたことを示しています。このコードがコンピュータに何を実行させようとしているか、少し考えてみてください。そうです。無限の数のコーラスのサンプルを0秒の間に再生するように指示してるのですね。もしセーフティシステムが無ければ、コンピュータはこれを実行しようして壊れてしまうでしょう。そんな訳で、`live_loop`には必ず`sleep`を含めることを覚えておいてください。


## 音を混ぜ合わせる

音楽は同時に発生する事象で溢れています。ギターとボーカルが同時に鳴っているところにベースが同時に鳴り、それらと同時にドラムが鳴るといったように…計算機の世界ではこれを*並行性*と呼び、Sonic Piも驚異的にシンプルな方法で同時に演奏することができます。単純に1つより多い`live_loop`を使えばよいのです。

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

ここでは、2つの`live_loop`を使っています。1つ目はせわしなくビートを刻み、もう1つはゆっくりとクレイジーなベース音を奏でています。

複数の`live_loop`を使った場合に興味深いのは、それぞれのループがそれぞれの時間を管理しているということです。これは、とても簡単にポリリズム的な構造を作ることを意味していて、スティーヴ・ライヒのような位相のずれを演奏することも簡単にできます。以下を見てください。

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

## まとめ

これらの個々のチュートリアルで紹介したアイデアの全てから引き出された新しい楽曲の例で終わりにしたいと思います。以下のコードを読んで、何をしているか想像してみてください。次に、それをSonic Piの新しいBufferにコピーし、`Run`を叩いて実際にどんな音がするか聞いてみましょう。最後に、どれかの数値を変更したり、コードをコメントアウトしたりコメントインしたりしてみましょう。そして、このコードが新しいパフォーマンスの開始点として使えるか見てみましょう。これらはとても楽しいと思います！ ではまた会いましょう。

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
