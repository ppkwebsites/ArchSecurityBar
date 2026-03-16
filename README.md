# Arch SecurityBar

Put the SecurityBar.sh script path in your hyprland.conf file as keybinding

bind = SUPER, S, exec, ~/.config/quickshell/SecurityBar/SecurityBar.sh


!!!! Troubleshooting SecurityBar
If the Security Dashboard fails to launch or displays an error regarding missing modules, follow these steps to resolve the issue.

1. Missing "Quickshell.Networking" Module
If you see the error module "Quickshell.Networking" is not installed, it means your version of Quickshell was not compiled with networking support. On Arch-based systems like CachyOS, you must build the package from source to ensure it detects your local networking libraries.

The Fix: Manual Source Build
Run the following commands in your terminal to compile Quickshell with the necessary plugins:

Bash
# 1. Install required build dependencies
yay -S cpptrace vulkan-headers networkmanager libnm

# 2. Clone and build Quickshell from the AUR
mkdir -p ~/builds && cd ~/builds
git clone https://aur.archlinux.org/quickshell-git.git
cd quickshell-git
makepkg -si
Verification:
After installation, verify the module exists by running:
ls /usr/lib/qt/qml/Quickshell/Networking/

2. Layout is "Screwed Up" or Misaligned
If the bar appears but the fonts, spacing, or alignment look incorrect, it is likely a rendering backend conflict. SecurityBar is designed to be pixel-perfect using the software renderer to match other system OSDs.

The Fix: Force Software Backend
Always launch the script or the QML file with the QT_QUICK_BACKEND=software environment variable:

Terminal Command:

Bash
QT_QUICK_BACKEND=software quickshell -p ~/.config/quickshell/SecurityBar/SecurityBar.qml
3. Script Does Not Open (Keybind Issues)
If pressing your keybind (e.g., SUPER + S) does nothing:

Check Executable Permissions:
Ensure the toggle script can run:
chmod +x ~/.config/quickshell/SecurityBar/SecurityBar.sh

Verify NetworkManager:
The script will exit silently if the service isn't active. Ensure it is running:
sudo systemctl enable --now NetworkManager

Path Mismatch:
Ensure the path in your hyprland.conf matches the actual location of the script:
bind = SUPER, S, exec, ~/.config/quickshell/SecurityBar/SecurityBar.sh

4. Notifications Don't Disappear
If error notifications stay on the screen indefinitely, ensure your notification daemon (Dunst) is configured correctly or that the script is passing a timeout flag.

Manual Fix for Dunst:
Edit ~/.config/dunst/dunstrc and set the timeout:

Ini, TOML
[global]
    timeout = 10
Then restart Dunst: killall dunst; dunst &

Dependency Checklist
Ensure these packages are installed for full functionality:

quickshell-git (Built from source)

networkmanager (Service must be enabled)

dunst (For error alerts)

ufw (For firewall status reporting)

clamav (For malware status reporting)

wl-clipboard (For the "Copy Sync Command" button)
