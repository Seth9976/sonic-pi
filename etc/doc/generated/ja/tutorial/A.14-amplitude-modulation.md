A.14 振幅変調

# 振幅変調

今月は、Sonic Piで最もパワフルでフレキシブルなエフェクトの1つである、`:slicer`を深く見ていきたいと思います。この記事の終わりまでに、ライブコーディングされたサウンドの全体的な音量を操作する、新しいパワフルな方法を習得できるでしょう。これは、新しいリズム的・音色的な構造を生成することができ、また音響的な可能性を広げることができるでしょう。

## あの音量をスライスしよう

実際のところ、`:slicer`エフェクトは何をやってるのでしょうか？ 誰かがテレビやオーディオ製品のボリュームコントロールで遊んでいるのと同じようなものと考えてみるのも、1つの方法かもしれません。早速見ていきたいところですが、最初に`:prophet`をトリガーする次のコードの深い唸り声を聞いてみてください：

```
synth :prophet, note: :e1, release: 8, cutoff: 70
synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
```

では、これを`:slicer`エフェクトに通してみましょう：

```

with_fx :slicer do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

`:slicer`が規則的なビートで音をミュートしたりしなかったりするのを聞いてみてください。また、`:slicer`が`do`/`end`ブロックの中で生成された全ての音に影響していることに注目してください。音のオン・オフの速さは、`phase duration`の短縮語である`phase:`オプションを使ってコントロールできます。`phase:`オプションのデフォルト値は`0.25`で、デフォルトのBPM60だと、1秒間に4回という意味になります。もっと速くしてみましょう：

```
with_fx :slicer, phase: 0.125 do
  synth :prophet, note: :e1, release: 8, cutoff: 70
  synth :prophet, note: :e1 + 4, release: 8, cutoff: 80
end
```

では、異なる`phase:`の長さで遊んでみましょう。長い値や短い値を試してみましょう。本当に短い値を選んだら何が起こるか見てみてください。また、`:beep`や`:dsaw`といった異なるシンセや、異なる音符も試してみましょう。次の図で、異なる`phase:`の値が、1拍の間に音の大きさを変更する回数をどのように変化させるか見てみましょう。

![Phase Durations](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_phase_durations.png)

`phase duration`は、オン・オフの周期の時間の長さを示してします。したがって、より小さい値はエフェクトのオン・オフをより早く切り替えます。はじめに試してみるのに良さそうな値は、`0.125`, `0.25`, `0.5`, `1`でしょう。


## 波形を操作する

デフォルトでは、`:slicer`エフェクトは音の大きさを操作するのに矩形波を使っています。一定時間音量がオンで聞こえてその後急にオフになるのは、これが理由です。矩形波は、`:slicer`がサポートしている4つの制御波形の1つであることが分かるでしょう。それ以外のものは、ノコギリ波、三角波、正弦波（または余弦波）です。下の図はそれぞれがどのような形をしているか示しています。また、それがどのような音がするのか聴くこともできます。例えば、次のコードは正弦波（または余弦波）を制御波形として使用しています。音が急にオン・オフせずにスムーズにフェードイン・フェードアウトするのを聴いてみてください。

```
with_fx :slicer, phase: 0.5, wave: 3 do
  synth :dsaw, note: :e3, release: 8, cutoff: 120
  synth :dsaw, note: :e2, release: 8, cutoff: 100
end
```

`wave:`オプションを変更することで、異なる波形で演奏してみましょう。`0`がノコギリ波、`1`が矩形波、`2`が三角波で、`3`が正弦波です。 また、異なる`phase:`オプションを組み合わせて、それぞれの波形がどのような音がするか試してみましょう。

これらの波形は、`invert_wave:`オプションでy軸方向に反転させることができます。例えば、典型的なノコギリ波は、高い値から始まり、徐々に値が下がっていって、ある時点で元の高い値に戻ります。`invert_wave: 1`を指定することで、低い値から始まり、徐々に値が上がっていって、ある時点で元の低い値に戻るようになります。また、波形操作は、`phase_offset:`オプションに`0`から`1`の値を指定することにより、波形の任意の位相から開始することができます。`phase:`, `wave:`, `invert_wave:`と`phase_offset`オプションを変更することで、時間に沿って音量をどれだけ変化させるかを、劇的に変更することができます。

![Phase Durations](../../../etc/doc/images/tutorial/articles/A.14-amplitude-modulation/slicer_control_waves.png)


## 音量を設定する

デフォルトでは、`:slicer`エフェクトは`1`（最大）と`0`（無音）の間の音量を切り替えます。これは、`amp_min:`と`amp_max:`オプションで変更できます。これを正弦波の設定に適用することで、シンプルなトレモロ効果を作ることができます：

```
with_fx :slicer, amp_min: 0.25, amp_max: 0.75, wave: 3, phase: 0.25 do
  synth :saw, release: 8
end
```

これはちょうど、オーディオ機器のボリュームつまみをひねって、音を`グラグラ`させるのに似ています。


## 確率

`:slicer`エフェクトのパワフルな特長の1つは、スライサーをオン・オフするかしないかを選択するのに、確率を使用可能なことでしょう。`:slicer`エフェクトが次の位相を開始する前にサイコロを振って、その結果をもとに選択した波形を使うかもしくは音量をオフにしたままにするかします。聞いてみましょう：

```
with_fx :slicer, phase: 0.125, probability: 0.6  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

興味深いパルスのリズムを生成できたのを聞くことができるでしょう。試しに、`probability:`オプションを`0`から`1`の間の値に変更してみましょう。`0`に近い値にすると、音がトリガーされる確率がより低くなるため、それぞれの音の間が広がるでしょう。

他にお伝えしたいのは、`:slice`エフェクトにおけるランダム化システムは、`rand`や`shuffle`といった関数経由でアクセス可能なランダム化システムとちょうど同じであるということです。これらはどちらも完全に決定的です。これは`Run`を叩いたときに、指定された確率に対応して毎回同じパルスのリズムを聞くことになることを意味しています。これらを変更したい場合には、`seed:`オプションを使って異なるシードを選択することが可能です。これは`use_random_seed`と全く同じように動作しますが、特定のエフェクトにのみ影響します。

最後に、確率的にオフになった場合に制御波形内の'休止する'位置を、`prob_pos:`オプションを使って`0`から他の値に変更できます：

```
with_fx :slicer, phase: 0.125, probability: 0.6, prob_pos: 1  do
  synth :tb303, note: :e1, cutoff_attack: 8, release: 8
  synth :tb303, note: :e2, cutoff_attack: 4, release: 8
  synth :tb303, note: :e3, cutoff_attack: 2, release: 8
end
```

## ビートをスライスする

`:slicer`エフェクトでドラムビートをブツ切りするのは、やってみると本当に楽しいことの1つです：

```
with_fx :slicer, phase: 0.125 do
  sample :loop_mika
end
```

これにより、任意のサンプル音源から新しいリズムの可能性を作り出すことができ、とても楽しいでしょう。しかしながら、1つ注意すべきことは、サンプルのテンポがSonic PiのカレントのBPMに合うようにすることです。そうでないと、スライスは全くの無音にしてしまうかもしれません。たとえば、`:loop_mika`を`loop_amen`サンプルに変更してみて、テンポが揃ってない場合にどれだけひどい音になるか聞いてみましょう。

## テンポを変更する

既に見てきたように、`use_bpm`でデフォルトのBPMを変更すると、全てのsleepの長さとシンセのエンベロープの長さがビートに一致するように伸縮されます。`:slicer`エフェクトの`phase:`オプションな実際には秒単位でなく拍単位なので、同じようにこれを受け入れることができます。したがって、BPMを変更してサンプルに一致させることで、上の`loop_amen`の問題を解決できます。

```
use_sample_bpm :loop_amen
with_fx :slicer, phase: 0.125 do
  sample :loop_amen
end
```

## まとめ

最後に、これらのアイデアを全て適用して1つの例にしましょう。`:slicer`エフェクトのみを使って、興味深い組み合わせを作り出してみましょう。ここから変更して自分自身の作品にしてみましょう！

```
live_loop :dark_mist do
  co = (line 70, 130, steps: 8).tick
  with_fx :slicer, probability: 0.7, prob_pos: 1 do
    synth :prophet, note: :e1, release: 8, cutoff: co
  end
  
  with_fx :slicer, phase: [0.125, 0.25].choose do
    sample :guit_em9, rate: 0.5
  end
  sleep 8
end
live_loop :crashing_waves do
  with_fx :slicer, wave: 0, phase: 0.25 do
    sample :loop_mika, rate: 0.5
  end
  sleep 16
end
```




