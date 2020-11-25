---
layout: post
title:  "Настройка Ubuntu"
crawlertitle: "This is our first post"
summary: "Настраиваем Ubuntu"
date:   2020-06-22 23:09:47 +0700
categories: posts
tags: ['Дата аналитика']
#author: Felipe
---


Первое, что необходимо сделать после установки Ubuntu или любого другого дистрибутива Linux, это обновить его. Обновлять систему Ubuntu необходимо не только после того, как только что установили данную операционную систему на свой компьютер, но и периодически. Это нужно делать, чтобы получать исправления багов, обновления безопасности и т.д. А также, чтобы актуализировать системные пакеты, которые зачастую требуются, как зависимости для установки сторонних программ.

# Обновления системы:

{% highlight python %}
sudo apt update && sudo apt upgrade
{% endhighlight %}


# Установка Snapy

Snap-пакет — это пакет, который помимо готовой сборки самого приложения, включает в себя все необходимые зависимости и может работать (почти) в любом дистрибутиве Linux.

{% highlight python %}
sudo apt update
sudo apt install snapd
{% endhighlight %}


# Смена сочетания клавиш 

Менять будем используя утилиту Gnome Tweaks

{% highlight python %}
sudo apt install gnome-tweaks
{% endhighlight %}

Запустите утилиту Gnome Tweaks. Запустить можно из Лаунчера (иконка "Доп. настрой...").

Выберите вкладку Клавиатура и мышь и нажмите кнопку Дополнительные параметры раскладки.

Откроется окно с разворачивающимся списком настроек комбинаций клавиш. Найдите пункт Переключение на другую раскладку. 
Установите галочку напротив сочетания, которое вы хотите использовать для переключение раскладки клавиатуры.
  
  
  
# Установка дополнительных пакетов

## Skype
{% highlight python %}
sudo snap install skype --classic
{% endhighlight %}

## Spotify
{% highlight python %}
sudo snap install spotify
{% endhighlight %}

## VLC
{% highlight python %}
sudo snap install vlc
{% endhighlight %}

## Krita
{% highlight python %}
sudo snap install krita
{% endhighlight %}

## Pycharm CE
{% highlight python %}
sudo snap install pycharm-community --classic
{% endhighlight %}

## Gimp
{% highlight python %}
udo snap install gimp
{% endhighlight %}

## Gitkraken
{% highlight python %}
sudo snap install gitkraken --classic
{% endhighlight %}

## Android-studio
{% highlight python %}
sudo snap install android-studio --classic
{% endhighlight %}

## Inkscape
{% highlight python %}
sudo snap install inkscape
{% endhighlight %}

## Sublime-text 
{% highlight python %}
sudo snap install sublime-text --classic
{% endhighlight %}

## Audacity
{% highlight python %}
sudo snap install audacity
{% endhighlight %}

## Postman
{% highlight python %}
sudo snap install postman
{% endhighlight %}

## Opera
{% highlight python %}
sudo snap install opera
{% endhighlight %}

## Telegram-desktop
{% highlight python %}
sudo snap install telegram-desktop
{% endhighlight %}

## Shotcut 
{% highlight python %}
sudo snap install shotcut --classic
{% endhighlight %}

## Chromium
{% highlight python %}
sudo snap install chromium
{% endhighlight %}

## Darktable
{% highlight python %}
sudo snap install darktable
{% endhighlight %}

## Zenkit
{% highlight python %}
sudo snap install zenkit
{% endhighlight %}

## Brave
{% highlight python %}
sudo snap install brave
{% endhighlight %}

## Electrum
{% highlight python %}
sudo snap install electrum
{% endhighlight %}

## Termius-app
{% highlight python %}
sudo snap install termius-app
{% endhighlight %}

## Miro 
{% highlight python %}
sudo snap install miro --edge
{% endhighlight %}

## Evernote
{% highlight python %}
sudo snap install evernote-web-client
{% endhighlight %}

## Slack
{% highlight python %}
sudo snap install slack --classic
{% endhighlight %}
