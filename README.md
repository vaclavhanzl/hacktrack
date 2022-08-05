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

## Comparison to other projects
I tried whatever I found before writing the tool myself, of course. Rather surprisingly, the key decision which allowed me to make this code simple and ergonomic was to NOT use library like libinput which is there to help me. Instead, I get raw trackpad events directly from the kernel. Sounds scary but is actually very easy. The problem with the (great) libinput is that it does only the things it can do well. It is not a place for experiments. If you want to be on the edge with crazy ideas, you need your experimental code PLUS a hacked version of libinput. This forked hacked libinput gets quickly unmaintained and it clashes with the libinput in your system. On the other hand, working in parallel with libinput (and anything like libinput-gestures on top of it) is really hassle free.

## Four fingers move
Four fingers can move in many ways. The tool just computes "center of gravity" (average position) of all four fingers and as this average point moves, the window moves. Use your creativity to find out how to use it with your hands. For example:
* use 4 fingers moving in unisono (the obvious way)
* put 3 fingers steady on the trackpad and add moving thumb. The window moves 4 times slower this way.
* put 3 fingers of one hand steady and move window with one finger of the other hand. Or slightly roll this touching finger for ultra-precise move.

## Five fingers resize
Just spread/shrink 5 fingers touching the trackpad and you will certainly figure it out. But for the curious, the algorithm looks for a bounding rectangle which contains your touch points. Relative changes of this rectangle's width and height then change width and height of the window. Again, you can get creative in many ways:
* just put all 5 fingers of one hand relaxed on the trackpad and then reshape
* put thumb in one corner of the bounding box and remaining 4 fingers in the diagonally oposite corner, then move these two corners
* put 4 fingers of one hand in a vertical or horizontal line. Add one finger of the other hand at certain distance from this line and move it. This way you can easily change just one dimension of the window.
* combine move and resize - add thumb or one finger of the other hand to switch from move to resize, than release it again and continue move

All this is actually fun to experiment with, you will certainly find a way you like.

## Three fingers desktop change
This one is actually just boring port of functionality I like on Mac. You can also get it easily with libinput-gestures so you may want to just disable it in hacktrack by changing this:
````python
@event_eater
def three_finger_desktop_drag():
````
into this (comment out the decorator):
````python
#@event_eater
def three_finger_desktop_drag():
````
If you want to use it, you need to match your desktop layout and the source code. It is no rocket science. My desktop is Xfce4 and I have a row of virtual desktops in a panel which appears when mouse pointer is at the lower edge of my display. When the pointer is elsewhere, the panel autohides. This part of the hacktrack source code forms the interface to my desktops:
```python
def desk_show():
    os.system("xfconf-query -c xfce4-panel -p /panels/panel-2/autohide-behavior -s 0")
def desk_hide(): # still shown if mouse at lower screen edge
    os.system("xfconf-query -c xfce4-panel -p /panels/panel-2/autohide-behavior -s 2")
def desk_left(): # like swipe-left on Mac (windows move left, our view moves right)
    os.system("xdotool key alt+ctrl+Right")
def desk_right():
    os.system("xdotool key alt+ctrl+Left")
```
* it changes the autohide property of ```panel-2``` to show/hide it. Your pannel can have different number.
* it uses ```alt+ctrl+Right``` and  ```alt+ctrl+Left``` to change desktops. You may need some other keys.

Also, you may use other commands than ```xfconf-query``` and ```xdotool``` to send instructions to your desktop.

## Absolute pointer positioning by tap
This one is really experimental and is disabled in the code:
```python
#@event_eater
def abs_pos_mouse_by_tap():
```
Remove the ```#``` to activate it. Also, unless your monitor happens to be 4K as the mine is, put the right size here:
```python
desktop_range = ((0, 3840), (0, 2160)) # x, y
```
Then you can tap the trackpad (slightly less than what would make it to click) at any point and the mouse cursor will appear at the corresponding absolute position of your screen. It is a neat way to find your cursor (or in fact not to search for it but place it where you want it) but I did not like it as much as the other features.

## Your own crazy idea
This is why you came here, right? It is really easy. The ```@event_eater``` decorator puts your procedure among those who recieve slightly cooked trackpad events (if you ever worked with mouse moving events, this is a similar thing). Try this:
```python
@event_eater
def eater():
    while True:
        x = (yield)
        print(x)
```
and look what is printed and than do something useful instead of the print. The code is full of examples of all the idioms you might need. Enjoy!

## Warning
Your window environment may contain elements which technically could be moved/resized by hacktrack but you do not want it. Code tries to avoid moving these:
```python
   if str(name).startswith("b'Desktop'"):
       # we are technically able to move this but do not want to
       print("Avoided Desktop drag")
       continue
```
and it works good enough for me but can be different for you.

## Contact
If you want to tell me that you love this or hate this, message @vaclav512 on Twitter.

