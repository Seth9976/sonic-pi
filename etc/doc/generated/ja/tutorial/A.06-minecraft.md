A.6 音楽的なMinecraft

# 音楽的なMinecraft


Minecraft Pi（マインクラフトパイ）


こんにちは、そしてまたお会いしましたね！ 前回のチュートリアルでは、純粋にSonic Piの音楽的な可能性に焦点を当てました（Raspberry Piをパフォーマンスに十分対応可能な楽器にしました）。ここまでで、我々は以下のことを学びました：

* ライブコーディング - 音楽を止めずに変更する、
* 大規模なビートをコードする、
* 強力なシンセリードを生成する、
* 有名なTB-303のアシッドベースサウンドを再生成する。

他にもたくさん紹介したい内容はあります（いずれこのチュートリアルで掘り下げる予定です）が、今月は、Sonic Piで出来ることのうち、おそらくあなたが知らないものを紹介したいと思います。Minecraftの制御です。

## Minecraftのワールドへようこそ

では始めましょう。Raspberry Piをブートし、Minecraft Piを起動して新しいワールドを作りましょう。そして、Sonic Piを起動し、Sonic PiとMinecraft Piの両方が見えるように、ウィンドウをリサイズしたり移動したりしましょう。

新しいBufferに次のコードを入力してみましょう。

```
mc_message "Hello Minecraft from Sonic Pi!"
```
    
では、`Run`を叩いてみてください。ブーン！ メッセージがMinecraftに表示されましたね！ とても簡単だったでしょう？ では、チュートリアルを読むのを止めて、少しの間メッセージを変更して遊んでみましょう。楽しでみて！

![Screen 0](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-0-small.png)

## Sonic Teleporter

では、少し探検してみましょう。ワールドで移動する標準的なやりかたは、マウスとキーボードを使って、あたりを歩き始めることです。これは機能しますが、かなり遅いし退屈でしょう。何か瞬間移動マシンのようなものがあると良いのですが。。ええ、Sonic Piのおかげで、それは実現できます。これを試してみてください。

```
mc_teleport 80, 40, 100
```
    
びっくりした！ 遠くまで移動できたでしょう。もし飛行モードなかったら、全ての行程を戻って地面に落ちてしまうでしょう。スペースキーをダブルタップして飛行モードに入ってもう一度瞬間移動すると、指定した場所に移動して浮いているでしょう。

では、これらの数字は何を意味するのでしょうか？ ワールド内の行きたい場所の座標を表現するのに、3つの数字を使っています。そしてそれらに名前を付けています。x, yそしてzです。

* x - どのくらい右端から左に離れてるか（我々の例では80）
* y - どのくらい高くあがったか（我々の例では40）
* z - どのくらい手前から奥へ離れてるか（我々の例では100）

異なるx, y, zの値を選ぶことで、ワールドの中の*どこでも*瞬間移動することができます。試してみよう！ 違う値を選んで、どこに辿り着いたか見てみましょう。もし画面が真っ黒になってしまったら、それは地面の下か山の中に瞬間移動してしまったからです。地面よりも上に戻るように大きなyの値を選択してください。何か発見するまで探検を続けましょう…

ここまでのアイデアを使って、Minecraftのワールドを瞬間移動するときの音を面白くする`Sonic Teleporter`を作ってみましょう。

```
mc_message "Preparing to teleport...."
sample :ambi_lunar_land, rate: -1
sleep 1
mc_message "3"
sleep 1
mc_message "2"
sleep 1
mc_message "1"
sleep 1
mc_teleport 90, 20, 10
mc_message "Whoooosh!"
```
    
![Screen 1](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-1-small.png)

## 魔法のブロック

では、良い場所が見つかったと思いますので、何か作り始めてみましょう。あなたの慣れたやり方でもできますし、マウスを激しくクリックしてカーソルの部分にブロックを設置することも可能です。それ以外に、Sonic Piの魔法を使うことも可能です。これを試してみてください。

```
x, y, z = mc_location
mc_set_block :melon, x, y + 5, z
```

見てみてください！ メロンが空中に現れました！ ここでは何をしてるのでしょうか？ 少し時間を取ってコードを見てみましょう。1行目では、プレイヤーの位置を取得してx, y, zの変数に入れています。これらの対応する座標については上で説明しています。これらの座標は、次の行の`mc_set_block`関数で、指定したブロックを設置するのに使用されます。ブロックをより高い位置に設置するには、yの値に5を加えていたのを増やす必要があります。次にブロックの長い軌跡を作ってみましょう。

```
live_loop :melon_trail do
  x, y, z = mc_location
  mc_set_block :melon, x, y-1, z
  sleep 0.125
end
```

では、Minecraftを表示して、飛行モードであることを確認して（もし飛行モードなかったらスペースキーをダブルタップしましょう）、ワールド中を飛び回ってみましょう。後ろにメロンのブロックの可愛い軌跡を見ることができたでしょう。どんなに曲がりくねったパターンでも作れたと思います。

## Minecraftをライブコーディングする

このチュートリアルをここ数ヶ月読んできた人は驚いたかもしれません。メロンの軌跡はとてもクールだけど、先程の例の最もエキサイティングな部分はMinecraftでも`live_loop`を使えるということです！ 知らない方のために説明すると、`live_loop`は他のプログラミング言語が持っていない、Sonic Piの特別な機能です。`live_loop`は複数のループを同時に実行し、また実行している最中にそれを変更することができます。それは信じられないほど強力で、驚くほど楽しいものです。私は、Sonic Piを使ってクラブで音楽を演奏するときに`live_loop`を使います。DJはレコードを使い、私は`live_loop`を使います:-) 今日は、音楽とMinecraftをライブコーディングしたいと思います。

では始めましょう。先程のコードを実行し、メロンの軌跡を作ってみましょう。そして、そのコードを止めずに、ただ`:melon`を`:brick`に変更して`Run`を叩いてみてください。あら不思議、レンガの軌跡が作れたでしょう。なんと簡単なのでしょう！ それに合わせて何か気の利いた音楽はいかがでしょう？ それも簡単です。これを試してみてください。

```
live_loop :bass_trail do
  tick
  x, y, z = mc_location
  b = (ring :melon, :brick, :glass).look
  mc_set_block b, x, y -1, z
  note = (ring :e1, :e2, :e3).look
  use_synth :tb303
  play note, release: 0.1, cutoff: 70
  sleep 0.125
end
```
    
では、それが演奏されている間に、コードを変更してみましょう。ブロックのタイプを変更してみましょう。`:water`や `:grass`、そしてあなたの好きなブロックを試してみましょう。それに加えて、カットオフの値を`70`から`80`に変更し、そして`100`に上げてみましょう。楽しいよね？

## まとめ

![Screen 2](../../../etc/doc/images/tutorial/articles/A.06-minecraft/Musical-Minecraft-2-small.png)

ここまで見てきた全てに、ちょっとした魔法を追加して組み合わせてみましょう。瞬間移動とブロック設置の機能と音楽を組み合わせて、Minecraftのミュージックビデオを作ってみましょう。全てを理解できなくても気にせずに、ただ以下のコードをタイプして、いくつかの値を実行中に変更してみましょう。楽しんでください。ではまた…
    
```
live_loop :note_blocks do
  mc_message "This is Sonic Minecraft"
  with_fx :reverb do
    with_fx :echo, phase: 0.125, reps: 32 do
      tick
      x = (range 30, 90, step: 0.1).look
      y = 20
      z = -10
      mc_teleport x, y, z
      ns = (scale :e3, :minor_pentatonic)
      n = ns.shuffle.choose
      bs = (knit :glass, 3, :sand, 1)
      b = bs.look
      synth :beep, note: n, release: 0.1
      mc_set_block b, x+20, n-60+y, z+10
      mc_set_block b, x+20, n-60+y, z-10
      sleep 0.25
    end
  end
end
live_loop :beats do
  sample :bd_haus, cutoff: 100
  sleep 0.5
end
```
