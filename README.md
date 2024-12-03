# legacy-screenshare-helper

Small tray application to support screenshare in legacy software that have trouble with Wayland.

The legacy-screenshare-helper runs in your tray, and gives you the option to "Start screenshare". Upon doing that, the XDG Desktop Portal is used to set up a ScreenCast to give you the usual OS dialogs for sharing monitors or application windows. Then a somewhat hidden (sent to the back, unfocusable) window running on your xWayland server is opened with the content of the ScreenCast. When you are done, you can use the tray icon again (or the XDG Desktop Portal icon) to stop the ScreenCast.

The app is useful in the context of legacy communication software such as Zoom, Discord, Teams, etc., which may have broken or missing support for XDG Desktop Portal, but work well with x11 screensharing. Since the legacy-screenshare-helper opens a screenshare window on *xWayland* (rather than Wayland), it will be visible for x11 applications that then can use normal legacy x11 screensharing techniques.

## Quickstart

### Dependencies

Ubuntu 24.10
```
  sudo apt install python3-gi python3-dbus gstreamer1.0-pipewire Xlib gir1.2-appindicator3-0.1
```

### Install app
```
  git clone https://github.com/httk/legacy-screenshare-helper
```

### Start app
```
  legacy-screenshare-helper/bin/httk-legacy-screenshare-helper
```
Note the new icon in the tray.

### Usage

Run your legacy communication software in x11 mode (on xWayland running on top of Wayland). Realize that you need to share your screen via the software.

Click on the tray icon and select 'Start screenshare'.
Start the functionality to share screen in the software.
Find the window named "Legacy screenshare helper" and share that window.
Once you are done, click the tray icon and select 'Stop screenshare'.
