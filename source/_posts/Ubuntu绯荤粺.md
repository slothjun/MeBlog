---
title: Ubuntu系统
---
# 自安装程序的图标显示

进入 ～/.local/share/applications/目录，在此目录下创建一个程序名称.desktop的文件可以在应用显示程序中加入此程序

example：

```linux
(base) spiderbao@spiderbao-CW65S:~$ cd ~/.local/share/applications/
(base) spiderbao@spiderbao-CW65S:~/.local/share/applications$ ls
Anaconda.desktop  jetbrains-idea-1.desktop  jetbrains-idea.desktop  jetbrains-pycharm.desktop  jetbrains-toolbox.desktop  jetbrains-webstorm.desktop  mimeapps.list  mimeinfo.cache  Typora.desktop
(base) spiderbao@spiderbao-CW65S:~/.local/share/applications$ 
(base) spiderbao@spiderbao-CW65S:~/.local/share/applications$ cat Anaconda.desktop 
[Desktop Entry]
Encoding=UTF-8
Name=Anaconda
Comment=Anaconda
Exec=/home/spiderbao/anaconda3/bin/anaconda-navigator
Icon=anaconda_icon_128x128
Terminal=false
Type=Application
Categories=GNOME;GTK;Utility;TextEditor;
Name[zh_CN]=anaconda

```

icon的位置 在/usr/share/pixmaps下

example：

```Linux
(base) spiderbao@spiderbao-CW65S:/usr/share/pixmaps$ pwd
/usr/share/pixmaps
(base) spiderbao@spiderbao-CW65S:/usr/share/pixmaps$ ls -l an
anaconda_icon_128x128.png  anki.png                   anki.xpm                   
(base) spiderbao@spiderbao-CW65S:/usr/share/pixmaps$ ls -l anaconda_icon_128x128.png 
-rw-rw-r-- 1 spiderbao spiderbao 4212 4月  21 09:30 anaconda_icon_128x128.png
(base) spiderbao@spiderbao-CW65S:/usr/share/pixmaps$ 

```

