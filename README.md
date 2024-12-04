# legacy-screenshare-helper

Small tray application to support screenshare in legacy software that have trouble with Wayland.

This is a free-standing app developed as part of the desktop helper software for the high-throughput toolkit (*httk*) [https://httk.org](https://httk.org).

The legacy-screenshare-helper runs in your tray, and gives you the option to "Start screenshare". Upon doing that, the XDG Desktop Portal is used to set up a ScreenCast to give you the usual OS dialogs for sharing monitors or application windows. Then a somewhat hidden (sent to the back, unfocusable) window running on your xWayland server is opened with the content of the ScreenCast. When you are done, you can use the tray icon again (or the XDG Desktop Portal icon) to stop the ScreenCast.

The app is useful in the context of legacy communication software such as Zoom, Discord, Teams, etc., which may have broken or missing support for XDG Desktop Portal, but work well with x11 screensharing. Since the legacy-screenshare-helper opens a screenshare window on *xWayland* (rather than Wayland), it will be visible for x11 applications that then can use normal legacy x11 screensharing techniques.

## Installation

### Install dependencies

For Ubuntu 24.04, 24.10:
```
sudo apt install python3-gi python3-dbus python3-xlib gstreamer1.0-pipewire gir1.2-appindicator3-0.1
```

### Install
Install from GitHub to a place where you want it, e.g.,:
```
git clone --depth 1 https://github.com/httk/legacy-screenshare-helper ~/Tools/legacy-screenshare-helper
cd ~/Tools/legacy-screenshare-helper
```
If you want to, install it into your desktop environment:
```
mkdir -p ~/.local/share/icons/hicolor/256x256/apps
cp legacy-screenshare-helper.png ~/.local/share/icons/hicolor/256x256/apps/.
cp legacy-screenshare-helper.desktop ~/.local/share/applications/.
sed -i "s|^Exec=.*\$|Exec=\"$(pwd -P)/bin/legacy-screenshare-helper\" %U|" ~/.local/share/applications/legacy-screenshare-helper.desktop
gtk-update-icon-cache -f -t ~/.local/share/icons/hicolor
update-desktop-database ~/.local/share/applications
```
And if you want to, make it autostart when you log in:
```
cp legacy-screenshare-helper.service ~/.config/systemd/user
sed -i "s|^ExecStart=.*\$|ExecStart=\"$(pwd -P)/bin/legacy-screenshare-helper\" %U|" ~/.config/systemd/user/legacy-screenshare-helper.service
systemctl --user daemon-reload
systemctl --user enable legacy-screenshare-helper
systemctl --user start legacy-screenshare-helper
```

### Update

If you have installed it according to the above instructions, then all that should be needed is to pull the latest changes from GitHub:
```
cd systemctl --user start legacy-screenshare-helper
git pull --depth 1
```
(However: look under "Update" in the newer version to check if there are any additional instructions.)

### Uninstall

If you made it autostart, this is how you disable that:
```
systemctl --user disable legacy-screenshare-helper
rm ~/.config/systemd/user/legacy-screenshare-helper.service
systemctl --user daemon-reload
```
If you installed it into your desktop environment, this is how you remove it:
```
rm ~/.local/share/applications/legacy-screenshare-helper.desktop
rm ~/.local/share/icons/hicolor/256x256/apps/legacy-screenshare-helper.png
update-desktop-database ~/.local/share/applications
gtk-update-icon-cache -f -t ~/.local/share/icons/hicolor
```
After this you can just remove the folder in which you installed it, e.g.:
```
rm -rf ~/Tools/legacy-screenshare-helper
```

## Usage

### Manually start app
If you made it autostart, it should already be running in your tray.

If you added it to your desktop, you can start it through selecting it among your other apps.

You can of course also just run it manually:
```
~/Tools/legacy-screenshare-helper/bin/httk-legacy-screenshare-helper
```
You should see a new icon added to the tray.

### Usage

Run your legacy communication software in x11 mode (on xWayland running on top of Wayland). Realize that you need to share your screen via the software.

Click on the tray icon and select 'Start screenshare'.
Start the functionality to share screen in the software.
Find the window named "Legacy screenshare helper" and share that window.
Once you are done, click the tray icon and select 'Stop screenshare'.
