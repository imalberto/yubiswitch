# yubiswitch
# Overview
`yubiswitch` is an OSX status bar application to enable/disable a [Yubikey Nano or Neo](https://www.yubico.com/products/yubikey-hardware/) from Yubico.

Yubikey is the producer of the Yubikeys: an hardware  authentication device, designed to provide an easy to use and secure compliment to the traditional username and password.

By touching the exposed gold edge, a YubiKey Nano emits a One Time Password (OTP) as if it was typed in from a keyboard. The unique passcode is verified by a YubiKey compliant application.

![Yubikey Nano picture](/images/nano.jpg)

So far all looks great doesn't it? :D

```
flnurfrdjvfrlutthjtjvcbcrlbbnnuu
ejehlrlrclcllukjgehhrttbknnbjdfn
njlvvnherbjvnljdvvvnihrfikufjucr
jhgkhrubrnuchhhbhrugvbenrhkcvich
```

Whooops! You see? I brought my laptop (lid opened) with me for a walk to a meeting room holding it with my right hand touching the golden stripe and this caused the Nano to start sending random OTP passwords to my Vim session, and to the FB chat window I had opened with my wife, and right now she's been asking WTF I've been writing :P

This status bar app allows you to avoid sending those accidental OTP passwords by allowing you to enable or disable the yubikey using a convenient global keyboard hot key that you can configure yourself.

# Download
Download the latest version in DMG format from [github release page here](https://github.com/pallotron/yubiswitch/releases/).

# Running
This application needs to run with escalated privileges in order to exclusively grab the USB HID interface that drives the NANO/NEO-n Yubikey. Running the main app as root is a p.i.t.a. so Yubiswitch installs an helper daemon with root privileges which contains the logic to grab the USB HID interface, the main application talks to this daemon via XPC calls. When you start Yubiswitch for the first time it will ask for your user's password, this is expected to install the helper before mentioned.

# Integration with shell
The application supports two basic AppleScript commands:
- KeyOn
- KeyOff

You can switch your yubikey on and off using this basic osacript commands:

```
$ osascript -e 'tell application "yubiswitch" to KeyOn'
```

```
$ osascript -e 'tell application "yubiswitch" to KeyOff'
```

# Screenshots
Menu items in the status bar:

![Menu items screenshot](/images/screenshot-menuitems.png)

Preference window:

![Menu items screenshot](/images/screenshot-prefs.png)

# Known Issues
- This applicaiton works with two models of yubikey,

  the _YubiKey Nano_ and the _Yubikey Neo_. The Nano and the Neo both fit cleanly into your USB port.

- The app's default settings support the Nano. If you have the neo, go into the app's `Preferences` by clicking on the menu icon, then set the the `Product ID` to `0x0114`.
- This app only works with recent version of OSX because it relies on the Notification Centre. OSX 10.8.x and above would do it. Sorry about that.

# TODO and future plans
- [x] Make hotkey configurable via configuration window, currently it's static and it is cmd-Y. (This is done via ShortcutRecorder now)
- [x] Use NSNotificationCenter to notify other classes (ie AppDelegate and YubiKey) when user preferences are changed
- [x] Add "Start at login" feature
- [x] Support for basic AppleScript comomands: KeyOn/KeyOff
- [ ] Feature: lock computer when yubikey is removed (use IOServiceAddMatchingNotification in IOKit?)
- [x] Support more yubikeys nano on multiple USB slots
- [x] Better support for plug and unplug events (fixed with HID interface, dmg not published yet)
- [ ] Convert release process using [github's Release APIS]([https://github.com/blog/1645-releases-api-preview](https://github.com/blog/1645-releases-api-preview)) and [ChocTop](http://drnic.github.io/choctop/)

# How to create DMG for distribution
Note that this will change soon to be integrated with [github's Release APIS] ([https://github.com/blog/1645-releases-api-preview](https://github.com/blog/1645-releases-api-preview))

When you want to create a release:
- Tag repo with vx.y, ie `git tag -a -m 'comment' v0.2`
- Compile release app bundle in Xcode
- Run script: `cd dmg/ && bash createdmg.sh`
- Get the file at `/tmp/yubiswitch\_$VERSION.dmg` and distribute via release page as explained in [the github blog](https://github.com/blog/1547-release-your-software)

# Dependencies
`yubiswitch` uses ShortcutRecoder to implement global hot key and shortcuts recording in the the preference window.
- ShortcutRecorder is at : [https://github.com/Kentzo/ShortcutRecorder](https://github.com/Kentzo/ShortcutRecorder)
- I'll need to add it to your `yubiswitch` clone using git submodule using
- command:

```
git submodule add git://github.com/Kentzo/ShortcutRecorder.git
```

See [this helpful page](https://github.com/Kentzo/ShortcutRecorder) for instructions on how to set up your Xcode environment should you have any problem.

# Credits
Credits:
- Anton Tolchanov (@knyar), he originally wrote this in Python using PyObjC bridge. I decided to port this into Objective-C to learn the language when I found out that Carbon Event Manager libs have been removed from Python3. See [http://docs.python.org/2/library/carbon.html](http://docs.python.org/2/library/carbon.html)
- @postwait for the USD HID device code
