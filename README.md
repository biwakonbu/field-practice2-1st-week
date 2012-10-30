
１、ゲーム Window 作成

#-- ここから
# -*- coding: cp932 -*-

require 'dxruby'

# タイトルの指定
# 指定しなければ "DXRuby Application" となる
Window.caption = "TETRIS"

#-- 実行してみる

# 設定を行わなければ画面サイズは 640 * 480 になる
# 横幅指定
Window.width = 480

# 縦幅指定
Window.height = 720

Window.loop do


end
# ゲーム動作処理ここまで

#-- ここまで

２、タイトル画面の作成

・Window.loop do の上に下記のコードを書く

#-- ここから

# タイトル画像を title という変数に確保させる
title = Image.load "image/title.png"

# フォントサイズを Pixel で指定
font_px = Font.new 40

# 文字列 "START" の座標
START_POSITION = {
  "x" => (380/2)-50,
  "y" => (640/2)
}

# 文字列 "EXIT" の座標
EXIT_POSITION = {
  "x" => (380/2)-50,
  "y" => (640/2)+60
}

# メニューの座標を指定
menu_list = [START_POSITION, EXIT_POSITION]

# シーン選択肢用文字列
str_start = "ＳＴＡＲＴ"
str_exit = "ＥＸＩＴ"

# メニュー選択用矢印情報
arrow = {
  # "font" => "矢印に使う文字列"
  "font" => "=>",

  # "x" => x 座標
  "x" => START_POSITION["x"],

  # "y" => y 座標
  "y" => START_POSITION["y"]
}

#-- ここまで


・Window.loop do の下に下記のコードを書く

#-- ここから
    # タイトル用の画像を描画 (横座標, 縦座標, 表示画像) <- の括弧内の順番で指定する
    Window.draw (480-300)/2, (320-100)/2, title

    # 文字の描画 (横座標, 縦座標, 表示文字, 文字サイズ(Pixel 指定))
    Window.drawFont 380/2, 640/2, str_start, font_px
    Window.drawFont 380/2, (640/2)+60, str_exit, font_px
    Window.drawFont arrow["x"], arrow["y"], arrow["font"], font_px
#-- ここまで


３、カーソルキーの移動
・EXIT へ移動
　上の続き、Window.drawFnt~ の下に下記のコードを書く。

#-- ここから
    # ↓キーを押下すると EXIT を選択
    if Input.keyPush? K_DOWNARROW
      arrow["x"] = EXIT_POSITION["x"]
      arrow["y"] = EXIT_POSITION["y"]
    end
# ここまで

・START へ移動
　EXIT へ移動のコードの下に書く。

#-- ここから
    # ↑キーを押下すると START を選択
    if Input.keyPush? K_UPARROW
      arrow["x"] = START_POSITION["x"]
      arrow["y"] = START_POSITION["y"]
    end
#-- ここまで

・終了コマンドの作成
　START へ移動で書いたコードの下に書く。

#-- ここから
  if Input.keyPush? K_Q
    break
  end
#-- ここまで

４、メニューの選択
・メニューを選択できるようにする。
　ゲームのモードを決める変数を作成する。

Window.loop do の上に書く。

#-- ここから
scene = "TITLE"
#-- ここまで

  Window.loop do の下に書く。

#-- ここから
if scene == "TITLE"
#-- ここまで

　さらにSTART へ移動するコードの下に書く。

#-- ここから
    # 選択肢処理
    if Input.keyPush? K_RETURN

      # START を選択するとゲーム開始
      if arrow["y"] == START_POSITION["y"]
        # シーン切り替えの為、ボタン入力後の待ち時間
        sleep 1
        scene = "TETRIS"
      end

      # EXIT を選択するとゲーム終了
      if arrow["y"] == EXIT_POSITION["y"]
        break
      end
    end
  end
  # TITLE おわり
#-- ここまで

５、盤面の作成
・ゲームプレイ用のモードを作る
　# TITLE おわり の下に書く。

#-- 
  # TETRIS 開始
  if scene == "TETRIS"

  end
  # TETRIS 1 ゲーム分の処理ここまで
#-- ここまで

・ゲーム盤を作る。
  scene = "TITLE" の下に書く。

#-- ここから

BACKGROUND = Image.load "image/background.png"
BLOCK = Image.load "image/block.png"
WALL = Image.load "image/wall.png"

TETRIS_IMAGES = [WALL, BACKGROUND, BLOCK]

## ゲームマップ
# 20 x 10 の盤面 + 予備
tetris_map = [
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,1,1,1,1,1,1,1,1,1,1,0,1,1],
              [1,0,0,0,0,0,0,0,0,0,0,0,0,1,1],
              [1,0,0,0,0,0,0,0,0,0,0,0,0,1,1],
              [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]
             ]
#-- ここまで

　if scene == "TETRIS" の下に書く。

#-- ここから
    map = Marshal.load(Marshal.dump(tetris_map))
    # ゲーム画面の作成
    Window.drawTile(0, 0, map, TETRIS_IMAGES, 0, 0, 14, 24)
#-- ここまで