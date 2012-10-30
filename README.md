
�P�A�Q�[�� Window �쐬

#-- ��������
# -*- coding: cp932 -*-

require 'dxruby'

# �^�C�g���̎w��
# �w�肵�Ȃ���� "DXRuby Application" �ƂȂ�
Window.caption = "TETRIS"

#-- ���s���Ă݂�

# �ݒ���s��Ȃ���Ή�ʃT�C�Y�� 640 * 480 �ɂȂ�
# �����w��
Window.width = 480

# �c���w��
Window.height = 720

Window.loop do


end
# �Q�[�����쏈�������܂�

#-- �����܂�

�Q�A�^�C�g����ʂ̍쐬

�EWindow.loop do �̏�ɉ��L�̃R�[�h������

#-- ��������

# �^�C�g���摜�� title �Ƃ����ϐ��Ɋm�ۂ�����
title = Image.load "image/title.png"

# �t�H���g�T�C�Y�� Pixel �Ŏw��
font_px = Font.new 40

# ������ "START" �̍��W
START_POSITION = {
  "x" => (380/2)-50,
  "y" => (640/2)
}

# ������ "EXIT" �̍��W
EXIT_POSITION = {
  "x" => (380/2)-50,
  "y" => (640/2)+60
}

# ���j���[�̍��W���w��
menu_list = [START_POSITION, EXIT_POSITION]

# �V�[���I�����p������
str_start = "�r�s�`�q�s"
str_exit = "�d�w�h�s"

# ���j���[�I��p�����
arrow = {
  # "font" => "���Ɏg��������"
  "font" => "=>",

  # "x" => x ���W
  "x" => START_POSITION["x"],

  # "y" => y ���W
  "y" => START_POSITION["y"]
}

#-- �����܂�


�EWindow.loop do �̉��ɉ��L�̃R�[�h������

#-- ��������
    # �^�C�g���p�̉摜��`�� (�����W, �c���W, �\���摜) <- �̊��ʓ��̏��ԂŎw�肷��
    Window.draw (480-300)/2, (320-100)/2, title

    # �����̕`�� (�����W, �c���W, �\������, �����T�C�Y(Pixel �w��))
    Window.drawFont 380/2, 640/2, str_start, font_px
    Window.drawFont 380/2, (640/2)+60, str_exit, font_px
    Window.drawFont arrow["x"], arrow["y"], arrow["font"], font_px
#-- �����܂�


�R�A�J�[�\���L�[�̈ړ�
�EEXIT �ֈړ�
�@��̑����AWindow.drawFnt~ �̉��ɉ��L�̃R�[�h�������B

#-- ��������
    # ���L�[����������� EXIT ��I��
    if Input.keyPush? K_DOWNARROW
      arrow["x"] = EXIT_POSITION["x"]
      arrow["y"] = EXIT_POSITION["y"]
    end
# �����܂�

�ESTART �ֈړ�
�@EXIT �ֈړ��̃R�[�h�̉��ɏ����B

#-- ��������
    # ���L�[����������� START ��I��
    if Input.keyPush? K_UPARROW
      arrow["x"] = START_POSITION["x"]
      arrow["y"] = START_POSITION["y"]
    end
#-- �����܂�

�E�I���R�}���h�̍쐬
�@START �ֈړ��ŏ������R�[�h�̉��ɏ����B

#-- ��������
  if Input.keyPush? K_Q
    break
  end
#-- �����܂�

�S�A���j���[�̑I��
�E���j���[��I���ł���悤�ɂ���B
�@�Q�[���̃��[�h�����߂�ϐ����쐬����B

Window.loop do �̏�ɏ����B

#-- ��������
scene = "TITLE"
#-- �����܂�

  Window.loop do �̉��ɏ����B

#-- ��������
if scene == "TITLE"
#-- �����܂�

�@�����START �ֈړ�����R�[�h�̉��ɏ����B

#-- ��������
    # �I��������
    if Input.keyPush? K_RETURN

      # START ��I������ƃQ�[���J�n
      if arrow["y"] == START_POSITION["y"]
        # �V�[���؂�ւ��ׁ̈A�{�^�����͌�̑҂�����
        sleep 1
        scene = "TETRIS"
      end

      # EXIT ��I������ƃQ�[���I��
      if arrow["y"] == EXIT_POSITION["y"]
        break
      end
    end
  end
  # TITLE �����
#-- �����܂�

�T�A�Ֆʂ̍쐬
�E�Q�[���v���C�p�̃��[�h�����
�@# TITLE ����� �̉��ɏ����B

#-- 
  # TETRIS �J�n
  if scene == "TETRIS"

  end
  # TETRIS 1 �Q�[�����̏��������܂�
#-- �����܂�

�E�Q�[���Ղ����B
  scene = "TITLE" �̉��ɏ����B

#-- ��������

BACKGROUND = Image.load "image/background.png"
BLOCK = Image.load "image/block.png"
WALL = Image.load "image/wall.png"

TETRIS_IMAGES = [WALL, BACKGROUND, BLOCK]

## �Q�[���}�b�v
# 20 x 10 �̔Ֆ� + �\��
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
#-- �����܂�

�@if scene == "TETRIS" �̉��ɏ����B

#-- ��������
    map = Marshal.load(Marshal.dump(tetris_map))
    # �Q�[����ʂ̍쐬
    Window.drawTile(0, 0, map, TETRIS_IMAGES, 0, 0, 14, 24)
#-- �����܂�