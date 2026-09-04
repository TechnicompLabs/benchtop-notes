# Input Devices

## Touchpad / Kinetic Scrolling

The problem: on modern Linux environments, there is no clear responsibility for where scroll handling code belongs. Kinetic/inertial scrolling is handled differently than in macOS.

The scroll processing chain:
1. libinput (handling and redirecting input events)
2. Display server
3. Compositor
4. Window manager
5. App layer (every app: Firefox, GIMP, etc.)

Currently kinetic scrolling is implemented on the app layer — every app has to handle scrolling events manually. On macOS, kinetic scrolling and rubber banding are handled within the OS.

Opinion: the scrolling code could belong in the compositor, so that not every app developer has to write code to handle it, while still preventing unwanted effects like kinetic scrolling transfer between windows. Additionally, the kinetic scrolling approach is not configurable in GNOME — some touchpads/screens scroll too fast, some too slow.

### Apple's Algorithm (Reference)
Apple uses momentum * 0.95 per frame. While touch lasts, screen moves 1:1. On touch end, momentum = pixels_swiped / time_swiped. If pixels < 10 or time < 0.5, momentum is clamped to zero. Then each frame: multiply momentum by 0.95, move screen by that amount.

* https://news.ycombinator.com/item?id=39607747
* https://stackoverflow.com/questions/38619717/need-help-dissecting-and-recreating-the-perfect-scroll-easing-based-on-pastrykit
* https://medium.com/homullus/recreating-native-ios-scroll-and-momentum-2906d0d711ad

## Keyboard Shortcuts

### Idea: Modal Keys
* Window key for Window Management
    * Window key: launch overview
    * Window-tab: switch between windows
    * Window-left/right: tiling
    * Window-up/down: minimize/maximize
* Alt key for Environment management (all apps)
    * Alt key: open app view
    * Alt-tab: switch between applications
* Ctrl key for Running Application (control focused app)
    * Ctrl-tab: switch between application tabs
* Caps key for controlling terminal

### Mouse Actions
* Middle Click → Minimize
* Double Click → Maximize

### CLI Interface UI Paradigms
* Space shows Command Palette (on desktop for GUI or in modal-CLI apps)
* Mode displayed on screen

## Gaming Mouse Configuration
    sudo systemctl enable --now ratbagd
* Solaar — included for managing Logitech mice along with libratbagd

## Game Controllers

* 8BitDo Ultimate Software Online — 8BitDo's controller configuration tool (button/stick remapping, profiles, firmware updates), historically Windows/Android-only; the "Online" version is announced as browser-based, which would make it usable from Linux (likely via WebHID/WebUSB) without a native app. Relevant to the gaming controller-support story (pairs with the ublue controller udev rules in Gaming Mode.md).
    * https://www.reddit.com/r/linux/comments/1w0lxdh/8bitdo_announce_ultimate_software_online_to/ — link filed from title/announcement; thread not yet reviewed (Reddit blocks automated fetch). VERIFY whether it needs Chromium/WebHID and whether firmware flashing works from Linux.


---

## From legacy notes: Touchpad.md
It isn't... You can see this in an open source JavaScript implementation of kinetic scrolling by Apple called PastryKit[3] using a magic number momentum * 0.9.
The problem is, that on modern Linux environments, there is no clear responsibility for where scroll handling code belongs. Especially Kinetic / Inertial scrolling is handled way different than in macOS.
There is libinput (for handling and redirecting input events)
There is the display server
There is the compositor
There is the window manager
There is the app layer (every App, like Firefox, Gimp,
Currently kinetic scrolling is implemented on the App layer, every app has to handle the scrolling events manually to provide kinetic scrolling. This is not the case in macOS... the kinetic scrolling / rubber banding is handled within the OS.
In my opinion, the scrolling code could belong into the compositor, so that not every app developer has to write code to handle the scrolling, but still prevent unwanted effects like kinetic scrolling transfer between windows. Additionally, the kinetic scrolling approach is not configurable in Gnome... some touchpads / screens are scrolling way to fast, some are too slow...
https://news.ycombinator.com/item?id=39607747
After many hours of dissecting the algorithm, we concluded that Apple is in fact using magic numbers. And the magic number is: (drumroll) momentum * 0.95.
Basically, while the touch lasts, apple lets you move the screen 1:1.
On touch end Apple would get momentum by dividing number of pixels that the user had swiped, and time that the user has swiped for. If the number of pixels was less than 10 or time was less than 0.5, momentum would be clamped to zero.
Anyways, once the momentum (speed) was known to us, they would multiply it by 0.95 in every frame, and then move the screen by that much.
So idiotically simple and elegant, that it hurts. :)
https://stackoverflow.com/questions/38619717/need-help-dissecting-and-recreating-the-perfect-scroll-easing-based-on-pastrykit
I am certain that i am not the only person ever to feel amused by the fact that in 2017, in the era of UX, not every scroll is the same. On second thought, it may be a poor decision to standardize everything, and while you may argue that one type of scroll physics is better than other, this is in fact a question of opinion. And my boss’ opinion was that we need to implement a clone of iOS scroll physics into our Unity mobile app.
https://medium.com/homullus/recreating-native-ios-scroll-and-momentum-2906d0d711ad


---

## From legacy notes: Universal Keyboard Shortcuts.md

### Idea: Modal Keys
Window key for Window Management
    Window key launch overview
    Window-tab to switch between windows
    Window-left/right for tiling
    Window-up/down for minimize/maximize
Alt key for Evironment management (Manage all apps)
    alt key open app view
    alt-tab switch between applications
Ctrl key for Running Application (Control running/focused app)
	ctrl-tab switch between application tabs
Caps key for controlling terminal

## Keyboard Shortcuts
Middle Click -> Minimize
Double Click -> Maximize

### CLI Interface UI Paradigms
  Space Shows Command Palatte (On desktop for GUI or in modal-CLI apps)
  Mode Displayed on Screen
