---
title: Setup Shortcut for Opening Softwares on Windows
date: 2016-11-26 12:10:42
tags: [shortcut, Windows]
category: Windows
---

Used to use Mac and love its flexible, especially for opening software. Unfortunately, Windows is my stage now. I must find the way, if you have such an headache staff, follow me:
* Win+R to open the `Run` window and type `regedit`, **Enter**;
* Position to HKEY_LOCAL_MACHINE/SOFTWARE/Microsoft/Windows/CurrentVersion/App Paths
* Create new item by right click App Paths and name it like xxx.exe;
* Update the default value on the right side to your software's install path.
* It finishes now. Open the Software: Win+R --> xxx

Enjoy it.


