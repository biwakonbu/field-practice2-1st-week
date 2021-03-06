DXRuby を使う
=============

Window 作成
-----------

### DXRuby を使ってウィンドウを表示するために最低限必要なプログラムを学ぶ。

```ruby
# -*- coding: cp932 -*-
require 'dxruby'

Window.loop do

end
```

ただし、上記プログラムのみではウィンドウを表示するだけで、ゲームとしては成り立たない。

### ゲームがゲームとして成り立つためにはどのようなモノが必要か?

> プログラムを書く上で仕様を固める事は非常に大事な事なので、モノを作る前には必ず
> どのようなモノを作るのかをある程度決めてしまう。勿論途中で変更されることもある。


キャラクター (画像) の表示
--------------------------

ゲームに使用するキャラクター (画像) の表示方法を学ぶ。

### 画像データの読み込み方

```ruby
# 一つのファイルに一つの画像しか無いときに使う
# "file_to_path" には文字列でファイルの名前の保存されているフォルダの名前とファイルの名前を記述する
sample_image = Image.load("file_to_path")

# 一つのファイルに全てのサイズが同じである複数の画像がある場合に使う
# xcount には画像ファイルの中に何個の画像が横並びになっているかを記述する
# ycount は xcount の縦版を記述する
sample_image_array = Image.load_tiles("file_to_path", xcount, ycount)

```

ここでは画像データの読み込み方を紹介しているが、プログラミングの中で非常に大事な概念が
二つあるのでそれぞれの解説を行う。

- 変数
- 配列

上記の二つの項目はプログラミングでは不可欠といえる要素で、どちらもデータをプログラムの上で名前をつけて保存しておくために用いる。
こうすることで、単なるデータが人間から見て意味が分かりやすくなる利点がある。

> 例えば、画像データを扱う場合に良く image という名前を使う人が多いが、image では何の画像か
> 分かりづらいので、画像データの中身に合わせた変数名にすると良い。
> 何らかのキャラクタの画像ならば、そのキャラクタの名前を変数名に使うなど。

変数を登録 (プログラミングの世界では定義と言う。以後定義を使用。) するには、先頭の文字が小文字のアルファベットで始まる英数字と特定の記号の文字列に何か値を代入する。

```ruby
student_id = 1
```

上記のプログラムでは、student_id という変数を作成し、数値を代入している。
代入とは、変数にデータを記憶させることである。
その為、代入する度に変数の中身は変更される。

変数を使用する場合は、正しい場所に使用したい変数の名前をそのまま記述すると
その時点で記憶しているデータに置き換わって処理される。
正しい場所の判断は、変数が文字を記憶しているのか、数値を記憶しているのか等によって異なる。
その為、最初は難しいかもしれないが、常に自分が今何になにのデータを受け渡ししたいのかを考える事が重要。
。
配列は、変数が多数のデータを扱えるようになったモノである。
しかし、データの集合自体には名前をつけられるが、データ一つ一つは番号でしか管理することが出来ない。データは下記の通りに扱われる。

```ruby
sample_array = []      # 配列のサンプル(空の配列作成)
sample_array[0] = 1

print(sample_array[0]) #=> 1 と出力される(sample_array[0] の中身を出力)

sample_array[1] = 2
.                      # 以下同じように好きな数の配列を定義できる
.
.
print(sample_array)    #=> [1, 2, ...] 配列名だけを指定した場合、配列の全体と置き換わる

```

上記プログラムの通り、配列は ``[]`` によって定義される。
また、配列名の末尾に ``[数値]`` をつけることでその配列の指定した数値番目のデータを取得出来る。
因みに、Ruby を含め、多くのプログラミング言語では、配列の先頭は 0 である。
その為、1 が先頭だと勘違いしないように気をつけること。


### 画像データをゲーム上で扱いやすくする為に

DXRuby では、画像データを読み込んだままの状態ではゲーム上で (処理の都合上) 扱いにくいため、
今回は、面倒くさい処理を裏方に任せるメソッドを使うことにする。
勿論使わずに自分でその機能を作る事もできる。

####

```ruby
x = 0
y = 0

image_data = Image.load_tiles("../character.png", 4, 4)

# Sprite.new とはゲーム上でキャラクタを扱う為に使用するメソッドで、
# このメソッドによって作られたデータを使うことで、
# キャラクタの位置の変更や衝突の判定を簡単に行うことが出来る
sample_sprite = Sprite.new(x, y, image_data[0])

Window.loop do
  # Sprite.draw メソッドを使うと指定した Sprite データを画面に表示 (描画) することが出来る
  Sprite.draw(sample_sprite)
end
```

### Sprite データの処理 (画像データの画面内移動)

Sprite データは x, y という二つの座標データを持っている。
このデータは Sprite.new を実行した際に登録した二つの座標である。
画像が表示されている座標は Sprite データが持っている座標である。

因みに数学とは違い、座標の原点 0 は画面の一番左上である。
左上から右に X 軸プラス方向、下に Y 軸プラス方向となる。
Y 軸の扱いが数学のグラフとは異なるため注意が必要。

Sprite データの x, y 座標には Sprite データの変数名にそれぞれ ``.x`` ``.y`` をつけることでアクセスできる。
また、座標の上書きも出来るため、座標を少しずつずらすことでパラパラ漫画のようにキャラクタを移動させることが出来る。


```ruby
Window.loop do
  # sample_sprite という Sprite データの x 座標をプラス 1 する
  # += という記号の組み合わせは、左辺の値に右辺の値をプラスし、代入する意味を持つ
  # 他にも、-=, *=, /= などもある
  # += 1 の場合だと左辺の変数の値が 1 ずつ増加していく
  sample_sprite.x += 1

  # y も 1 ずつ増加
  sample_sprite.y += 1

  # Sprite.draw メソッドを使うと指定した Sprite データを画面に表示 (描画) することが出来る
  Sprite.draw(sample_sprite)
end
```

### キャラクタの操作

キャラクタの操作を行うために、キーボードの入力の受け付けを作る必要が出て来た。
DXRuby では、キーボード、マウス、ゲームパッドのそれぞれのアクション (要するにボタンを押したりする行為) を受け付けており、簡単に扱えるためこの機能を使ってキャラクタの移動を機能を作成する。

今回は、キャラクタの移動はキーボードから受け付けて行う。
キーボードの受付には、``Input.keyDown?`` というメソッドを使用する。
これは、メソッドの () 内で指定したキーの入力 (押し続けを含む) を真偽値で返す。
指定したキーを押していれば true という真をあらわすす値が返される。
指定したキーが押されていなければ false という偽をあらわす値が返される。

このメソッドだけでは、押しているのかどうかが分かるだけで、処理が変化しない (特定のボタンを押した時にキャラクタが移動しない) ため、条件分岐を使う。

条件分岐は、特定の条件を満たした時にだけ特殊な処理を実行するときに使用する。
今回の場合で言うと、キーボードの特定のキーを押すとキャラクタが移動する。
何も押していない場合は動かない、といった処理である。

```ruby
Window.loop do
  # Ruby の条件分岐は if `条件式` ~ end までの間に書く
  # K_L と言うのは、キーボードの L キーという意味
  # つまりキーボードの L のキーを押して (押し続けて) いたら true (真) である
  # true が返ってくると、if `条件式` ~ end の間に書いた処理が実行される
  # この場合は sample_sprite.x += 1 が実行される
  # 因みに、移動速度を変更したい場合は数値を変更すると良い
  if Input.keyDown?(K_L)
    sample_sprite.x += 1
  end

  # 後のキーの設定も行う
  if Input.keyDown?(K_K)
    sample_sprite.y -= 1
  end

  if Input.keyDown?(K_J)
    sample_sprite.y += 1
  end
  
  if Input.keyDown?(K_H)
    sample_sprite.x -= 1
  end

  # Sprite.draw メソッドを使うと指定した Sprite データを画面に表示 (描画) することが出来る
  Sprite.draw(sample_sprite)
end
```

### 衝突判定

衝突判定とは、オブジェクト(物体) とオブジェクトが
仮想的に衝突を起こしている条件を満たした場合に衝突が行われていることを判定する仕組み。
これが上手く動いていなかったりするとゲームキャラクタが壁をすり抜けるバグ等が発生したりする。

本来衝突判定は非常に面倒くさい処理を必要とする
 (皆の大嫌いな数学で、行列とベクトル辺りそれだけじゃないケド...) 
が、例によって簡単に判定できる仕組みがあるのでそちらを使用する。

まず衝突には二つ以上のオブジェクト (キャラクタ等のゲーム上に存在する物体) が必要なので、
Sprite データとして２つ用意してみる。
簡単に判定を行うには Sprite データとして扱わないと行けないので、
必ず Sprite データとして画像を用意すること。


```ruby
image_chara = Image.load_tiles("../image/character.png", 4, 4)
chara = Sprite.new(0, 0, image_chara[0])

image_box = Image.load_tiles("../image/colorbox.png", 6, 1)
box = Sprite.new(200, 200, image_box[0])

Window.loop do
  if Input.keyDown?(K_L)
    chara.x += 2
    # === を使って左辺と右辺を比較する事で衝突の判定をとることが出来る
    # この時両方のデータは Sprite データを持った変数か、配列であること
    # それ以外ではエラーとなる
    # 衝突が行われた場合は true が返ってくるため、この場合、
    # 直前にキャラクタの座標を x+2 しているため、その時に衝突した場合は
    # x+2 の処理を取りやめている
    # 結果的に、キャラクタは移動せずに壁にぶつかって止まっているように見える
    if chara === box
      chara.x -= 2
    end
  end

  # 後のキーの設定も行う
  if Input.keyDown?(K_K)
    chara.y -= 2
    if chara === box
      chara.y += 2
    end
  end

  if Input.keyDown?(K_J)
    chara.y += 2
    if chara === box
      chara.y -= 2
    end
  end

  if Input.keyDown?(K_H)
    chara.x -= 2
    if chara === box
      chara.x += 2
    end
  end

  # 画面の更新を行っている
  # この処理が実行されるまでは、オブジェクトにどれだけ変化が起こっていても
  # 見た目には反映されない
  Sprite.draw(chara)
  Sprite.draw(box)
end
```