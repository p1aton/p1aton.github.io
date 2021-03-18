---
layout: post
title:  "VirtualBox мануал"
crawlertitle: "This is our first post"
summary: "VirtualBox мануал"
date:   2020-06-29 23:09:47 +0700
categories: posts
tags: ['Дата аналитика']
#author: Felipe
---


Решил поломать себе вечный кайф танцев с бубнами вокруг установки VirtualBox и написать небольшой мануал. 

### Установка VirtualBox в Linux

В Linux VirtualBox может быть установлен несколькими способами:

*    из обычного репозитория
*    бинарным файлом, скаченным с официального сайта
*    из репозитория VirtualBox, добавленного в источники приложений (только для основанных на Debian дистрибутивов)


По этой [ссылке](https://www.virtualbox.org/wiki/Download_Old_Builds_6_1) можно выбрать необходимую версию. 

#### Установка VirtualBox в Debian и производные (Ubuntu, Linux Mint, Kali Linux)

{% highlight python %}
sudo apt install virtualbox virtualbox-qt linux-headers-"$(uname -r)" dkms vde2 virtualbox-guest-additions-iso vde2-cryptcab
{% endhighlight %}

#### Установка VirtualBox в Arch Linux и производные (BlackArch и другие)

{% highlight python %}
sudo pacman -S virtualbox linux-headers virtualbox-host-dkms virtualbox-guest-iso
{% endhighlight %}

### Установка Extensions Pack

Пакет расширений VirtualBox добавляет последующие функции

*    Виртуальное устройство USB 2.0 (EHCI)
*    Виртуальное устройство USB 3.0 (xHCI)
*    Поддержка протокола Удалённый Стол VirtualBox (VRDP)
*    Переброска веб-камеры хоста
*    Intel PXE boot ROM
*    Экспериментальная поддержка передачи PCI на хостах Linux hosts
*    Шифрование вида диска методом AES

Для просмотра установленных в настоящее время пакетов расширений, откройте основное VirtualBox Менеджер (главное окно программы), в меню «Файл» выберите «Настройки». В открывшемя окне перейдите во вкладку «Плагины», там вы увидите установленные в настоящее время расширения и можете удалить пакет либо добавить новый: 

![image tooltip here](/assets/images/12.jpg)

Пакет расширений для последней версии вы сможете отыскать на страничке скачки. 


### Установка Guest Additions

Для того чтобы VirtualBox масштабировал запущеную виртуальную машину по размеру вашей основной машины нужно правильно установить Guest Additions. Ключевое слово правильно здесь не случайно, так как именно на этом этапе у большинства пользователей возникают сложности и появляется ошибка от Guest Additions. 

##### Важно! Guest Additions устанавливается внутри запущенной виртуальной системы, а не на основной машине. 

Когда вы запустили вируальную машину с вашей системой зайдите в меню VirtualBox и смонтируйте гостевые дополнения. 

![image tooltip here](/assets/images/insert-guest-additions-cd-windows.jpg)

У вас на машине появится значок диска с гостевыми дополнениями зайдите в него и найдите VBoxLinuxAdditions.run и запустите данный файл.

Иногда хватает только запуска этого файла, но у меня все-равно не работало на Ubuntu, спасла эта команда.

{% highlight python %}
sudo apt install virtualbox-guest-utils virtualbox-guest-dkms
{% endhighlight %}

После чего еще раз запустите 

{% highlight python %}
sudo ./VBoxLinuxAdditions.run
{% endhighlight %}

