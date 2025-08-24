# mac-keyboard-short-cut-in-kali-linux-

Keyboard Remapping for Linux (XFCE) - Swap Control and Command Keys for Mac Keyboards
This repository provides a configuration to swap the Control (Ctrl) and Command (Cmd) keys on a Mac keyboard when used in a Linux system with XFCE or other X11-based desktop environments. This is useful for Mac users transitioning to Linux who prefer the familiar Command key behavior for Control functions.
Prerequisites

Linux system with X11 (not Wayland)
xmodmap installed (typically pre-installed on most distributions)
XFCE or a similar X11-based desktop environment with a "Settings Manager"
A Mac keyboard (e.g., Apple Magic Keyboard or MacBook keyboard)
Basic terminal knowledge

Step-by-Step Instructions
Step 1: Create the .xmodmap Configuration File
Mac keyboards typically map the Command key to the Super key (keycode 133 or 134) and the Control key to keycode 37 in Linux. The following commands create a ~/.xmodmap file to swap the left Control (keycode 37) and left Command (Super_L, keycode 133) keys.

Run these commands in your terminal:
```
echo "keycode 133 = Control_L NoSymbol Control_L" > ~/.xmodmap
echo "keycode 37 = Super_L NoSymbol Super_L" >> ~/.xmodmap
echo "clear mod4" >> ~/.xmodmap
echo "clear control" >> ~/.xmodmap
echo "add control = Control_L Control_R" >> ~/.xmodmap
echo "add mod4 = Super_L Super_R" >> ~/.xmodmap
```
Verify the file contents:
```
cat ~/.xmodmap
```

Note: Keycodes may vary depending on your specific Mac keyboard model. Use the xev tool to confirm keycodes if the defaults (133 and 37) don’t work (sudo apt install x11-utils to install xev).
Step 2: Add to Autostart
To apply the key swap automatically on login in XFCE:

Open "Settings Manager" (search in your menu).
Navigate to "Session and Startup" > "Application Autostart".
Click "Add" and enter:
Name: Xmodmap Mac Keyboard
Command: xmodmap /home/yourusername/.xmodmap (replace yourusername with your actual username, e.g., /home/john/.xmodmap).
Description: Load custom Mac keyboard mappings


Click "Save".

Step 3: Test the Configuration
Test the remapping without rebooting:
```
xmodmap ~/.xmodmap
```

Press the left Command key (should act as Control, e.g., try Cmd + C for copy).

Press the left Control key (should act as Super, e.g., try Ctrl + T in a window manager to test.)

If the swap doesn’t work, reset with:
```
xmodmap -e "keycode 133 = Super_L"
xmodmap -e "keycode 37 = Control_L"
```

Step 4: Reboot and Verify
Reboot to ensure the autostart configuration applies:

```
sudo reboot
```
After reboot, the key swap should take effect automatically. If not, manually run xmodmap ~/.xmodmap and check logs (e.g., journalctl -b -u lightdm).
Troubleshooting

Incorrect keycodes: Use xev to identify the correct keycodes for your Mac keyboard. Press the Command and Control keys in xev to see their keycodes.
Changes not persisting: Ensure the autostart command uses the full path to ~/.xmodmap (e.g., /home/yourusername/.xmodmap).
Wayland users: This method is for X11 only. For Wayland, explore tools like gnome-tweaks or evdev-remap-keys.
Revert changes: Delete the autostart entry in XFCE Settings Manager and remove ~/.xmodmap, then reboot.

Using xev to Find Keycodes
If the default keycodes (133 and 37) don’t work:

Install xev: sudo apt install x11-utils
Run xev in the terminal.
Press the left Command and Control keys. Note the keycodes displayed in the terminal.
Update the .xmodmap file with the correct keycodes, replacing 133 and 37 as needed.

Files in This Repository

.xmodmap: Configuration file with commands to swap Control and Command keys for Mac keyboards.

Contributing
Contributions are welcome! Fork this repository and submit pull requests with improvements, such as support for other desktop environments, Wayland compatibility, or additional Mac keyboard models.
License
This project is licensed under the MIT License. See the LICENSE file for details.
