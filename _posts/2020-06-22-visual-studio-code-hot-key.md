---
layout: post
title:  "Visual Studio Code горячие клавиши"
crawlertitle: "This is our first post"
summary: "VSCode горячие клавиши"
date:   2021-01-27 23:09:47 +0700
categories: posts
tags: ['Дата аналитика']
#author: Felipe
---


Горячие клавиши в VSCode. Набор всех доступных горячих клавиш (быстрых команд) для работы с редактором кода VSCode, которые значительно упрощают ведение разработки и написание кода.

#### Комментирование

CTRL+K & CTRL+C - быстрый способ комментирования
CTRL+K & CTRL+U - oбратное раскомментирование
CTRL + / - однострочное комментирование

#### Изменить все вхождения:

 1.   Сначала наведите курсор на элемент и нажмите F2.

 2.   Затем введите новое имя и нажмите клавишу Enter. Это переименует все вхождения в каждом файле вашего проекта.

 или второй вариант CTRL + H 

#### Автоимпорт 

 CTRL + пробел

#### Форматирование 

 CTRL+K & CTRL+F  
 CTRL+SHIFT+i  
 
 #### Редактирование и импорт плагинов
 
 

С текущей версией VSCode на момент написания (1.22.1) вы можете найти свои настройки в

    ~/.config/Code/User в Linux (в моем случае, производная от Ubuntu)
    C:\Users\username\AppData\Roaming\Code\User в Windows 10
    ~/Library/Application Support/Code/User/в Mac OS X (спасибо, Кристоф Де Тройер )

Файлы есть settings.jsonи keybindings.json. Просто скопируйте их на целевой компьютер.

Ваши расширения находятся в

    ~/.vscode/extensions в Linux и Mac OS X
    C:\Users\username\.vscode\extensions в Windows 10 (например, по существу, в том же месте)

Кроме того, просто перейдите к Расширениям, покажите установленные расширения и установите их в вашей целевой установке. Для меня копирование расширений работало просто отлично, но это может зависеть от конкретного расширения, особенно при перемещении между платформами, в зависимости от того, что делает расширение.

#### Открыть VS Code из командной строки

Открыть VSC команда code в терминале

Открыть файл code filename в терминале👽



 [Расширенный справочник по горячим клавишам VSCode](https://nikomedvedev.ru/other/vscodeshortcuts/hotkeys.html)  


 <!-- sudo openvpn --config 'Загрузки/vpn/VPNBook.com-OpenVPN-DE4/vpnbook-de4-tcp443.ovpn' -->
