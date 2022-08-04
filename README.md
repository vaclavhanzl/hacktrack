# hacktrack: Move windows by four fingers on trackpad, resize by five, and more...
Being spoiled by window management on both Linux and Mac, I want all the good things combined. Three-finger swipe to change desktop? Great. Alt+mouse anywhere to move/resize window? Much better than aiming at tiny GUI parts but do I really have to reach for both the keyboard and mouse? Four fingers on trackpad should move windows, right? After all, mouse feels evil being spoiled by Mac trackpad. So after a vain search for a system I could configure to respond really well, I wrote this little goody. And I absolutely love my window management now.

## Prerequisities
* **Apple Magic Trackpad 2.** This is an absolutely amazing piece of hardware, even much better than you guess from a Mac experience. Did you know it precisely detects up to 11 fingers? :) And shapes of their touch areas. And pressure.
* **Big screen.** Window management is not important on laptop when you work mostly fullscreen. Things are quite different on something like 31.5" EIZO EV3285.
* **Linux desktop like Xfce4.** You need to be able to programatically move windows etc. and most Linux desktops will let you do that.

## Features
* **Move windows by four fingers.** Just as easily as you scroll by 2 fingers and change desktop by 3, you can move window by 4.
* **Resize windows by five fingers.** Spread/shrink 5 fingers to change window's size and shape. It is amazingly easy and you can combine it with the 4 finger move - position window, add one more finger to resize it slightly, continue positioning...
* **Change desktop with three fingers.** You know this from Mac. But here you can tune it. What about longer swipe to go over several desktops instead of repeated swipes?
* **Whatever hands can do and brain wants.** Having direct access to all the data from your trackpad, slightly processed to an easy-to-work-with form, and having a modular structure of hacktrack, you can add more features on a few python lines.

## Instalation
* Install python
* ```pip install xdo```
* ```pip install evdev```
* make sure hacktrack is run when you log in to your GUI

For desktop changes, you will likely need to modify commands used in the hacktrack source according to your desktop setup.

## TODO
This is an alpha stage project. Some more automatic installation is certainly due.
