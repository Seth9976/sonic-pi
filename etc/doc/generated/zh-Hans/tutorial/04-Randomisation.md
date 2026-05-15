4 随机化

# 随机化

要让你的音乐变得更有趣，加入一些随机数是和好的办法。Sonic Pi有很多功能可以为你的音乐加入随机性，但开始之前我们要认清一个现实：Sonic Pi中*所有的随机并不是真的随机*！那到底是什么啊？让我们来看看。

## 可再现性

`rrand`是一个很有用的随机函数，它会在两个数字之间随机生成一个数，即*上限*和*下限*，（`rrand`是"ranged random"的缩写）。试一下演奏一个随机音符：

```
play rrand(50, 95)
```

看，出来了一个随机的音，相应midi音符是`83.7527`，是介于50和95之间的随机数。但是等等，难道我能预测出你得到的随机数吗？这个事情很可疑。试试再运行一次这段代码。什么？又是`83.7527`？这不是随机数啊！

这问题的答案就是这不是真的随机数，是"伪随机"（pseudo-random）。Sonic Pi用了一种可以再现的方式，给了你一个看起来像随机数的数。这个机制可以保证你在自己机器上创作的音乐在别人的机器上听起来也是一样的，即使你在创作中加入了随机性。

当然，在某段特定的音乐里，如果你的"随机数"每次都是`83.7527`，那肯定会很无聊。但实际上并不会。试试下面的代码：

```
loop do
  play rrand(50, 95)
  sleep 0.5
end 
```

对！这听起来很随机。在某次特定的*运行*中，先后使用随机函数得可以得到一系列随机数，但是下一次运行这段代码的时候，还会得到同样的一系列随机数，所以每次运行听起来都是一样的。就好像Sonic Pi每次运行都会回到你第一次按下"运行"按钮的那个时间点。去看看《今天暂时停止》（Groundhog Day, 1993）这部电影吧，这样的事情就发生在音乐合成中！

## 连绵不绝的钟声（示例代码：Haunted Bells）

示例代码中的haunted bells用非常可爱的方式演示了随机化，它用随机的采样率和间隔时间反复播放 `:perc_bell`这个采样：

```
loop do
  sample :perc_bell, rate: (rrand 0.125, 1.5)
  sleep rrand(0.2, 2)
end
```

## 随机截止频率

关于随机化另一个有趣的例子是随机设定一个合成器的截止频率，合成器`:tb303`模拟器是一个很好的实验对象：

```
use_synth :tb303
loop do
  play 50, release: 0.1, cutoff: rrand(60, 120)
  sleep 0.125
end
```

## 随机种子

如果你实在不喜欢Sonic Pi给你生成的这一组随机数怎么办？你完全可以通过`use_random_seed`选择另一个产生随机数的"种子"。默认的种子编号是0，选一个别的数字就可以体验到完全不一样的随机体验!

试试下面的例子：

```
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

你每次运行这段代码会听到同样的5个音符。如果你想要随机产生5个不一样的音符，换一个随机种子就可以：

```
use_random_seed 40
5.times do
  play rrand(50, 100)
  sleep 0.5
end
```

这段代码会生成完全不同的5个音符。多试试几个不同的随机种子，你总能找到让你满意的结果，而当你与别人分享代码时，他们会听到同样的随机结果。

我们再来看看另一些有用的随机函数。


## choose函数

我们经常要做的一件事就是从一些已知的元素中随机选择一个。比如说，我可能会想要在60,65和72这三个音中随机选一个来演奏。这个时候就可以通过`choose`来实现。首先我需要把这三个音放到一个列表中，用方括号把它们包起来，再用逗号做分隔即可（注意要用英文的方括号和逗号）：`[60, 65, 72]`，然后我只需要把这个列表交给`choose`:

```
choose([60, 65, 72])
```

Let's hear what that sounds like:

```
loop do
  play choose([60, 65, 72])
  sleep 1
end
```

## rrand

We've already seen `rrand`, but let's run over it again. It returns a random number between two values exclusively. That means it will never return either the top or bottom number - always something in between the two. The number will always be a float - meaning it's not a whole number but a fraction of a number. Examples of floats returned by `rrand(20, 110)`:

* 87.5054931640625
* 86.05255126953125
* 61.77825927734375

## rrand_i

Occasionally you'll want a whole random number, not a float. This is where `rrand_i` comes to the rescue. It works similarly to `rrand` except it may return the min and max values as potential random values (which means it's inclusive rather than exclusive of the range). Examples of numbers returned by `rrand_i(20, 110)` are:

* 88
* 86
* 62

## rand

This will return a random float between 0 (inclusive) and the max value you specify (exclusive). By default it will return a value between 0 and one. It's therefore useful for choosing random `amp:` values:

```
loop do
  play 60, amp: rand
  sleep 0.25
end
```

## rand_i

Similar to the relationship between `rrand_i` and `rrand`, `rand_i` will return a random whole number between 0 and the max value you specify.

## dice

Sometimes you want to emulate a dice throw - this is a special case of `rrand_i` where the lower value is always 1. A call to `dice` requires you to specify the number of sides on the dice. A standard dice has 6 sides, so `dice(6)` will act very similarly - returning values of either 1, 2, 3, 4, 5, or 6. However, just like fantasy role-play games, you might find value in a 4 sided dice, or a 12 sided dice, or a 20 sided dice - perhaps even a 120 sided dice!

## one_in

Finally you may wish to emulate throwing the top score of a dice such as a 6 in a standard dice. `one_in` therefore returns true with a probability of one in the number of sides on the dice. Therefore `one_in(6)` will return true with a probability of 1 in 6 or false otherwise. True and false values are very useful for `if` statements which we will cover in a subsequent section of this tutorial.

Now, go and jumble up your code with some randomness!
