---
tags:
  - Arch
  - Linux
---
# Установка через archinstall:

# Post-install:
## Другой формат хранения времени (чтоб не ломалось время при загрузке в винду)
```zsh title="terminal"
sudo timedatectl set-local-rtc 1 --adjust-system-clock
```
## Смена языка на Alt + Shift
```zsh title="terminal"
gsettings set org.gnome.desktop.wm.keybindings switch-input-source "['<Alt>Shift_L']";gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward "['<Shift>Alt_L']"
```
## Отображение восточно азиатских символов
```zsh title="terminal"
sudo pacman -S ttf-sazanami
```
## Yay(AUR):
```zsh title="terminal"
sudo pacman -S --needed base-devel git && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```
## Pamac(GUI для pacman, yay и flatpak)
```zsh title="terminal"
yay -S pamac-flatpak
```
# Основные программы:
## ОФ репозитории: 
```zsh title="terminal"
sudo pacman -S steam libreoffice-fresh nvidia-settings flatpak neofetch
```
## flatpak :
```zsh title="terminal"
flatpak install flathub org.telegram.desktop com.github.flxzt.rnote com.mattjakeman.ExtensionManager io.github.purplehorrorrus.Meridius md.obsidian.Obsidian com.discordapp.Discord com.dec05eba.gpu_screen_recorder com.github.tenderowl.frog io.github.vikdevelop.SaveDesktop
```
## yay(AUR): 
```zsh title="terminal"
yay -S visual-studio-code-bin brave-bin ttf-droid-sans-mono-dotted-powerline-git
```
# Extensions:
- X11 gestures 
- gSnap
- Custom Accent Colors
- Caffeine
- blur my shell
- User themes
- Vitals
- Adds AppIndicator / KStatusNotifierItem
- Clipboard Indicator
# Troubleshooting:
## VS думает что она nautilus:
```zsh title="terminal"
xdg-mime default org.gnome.Nautilus.desktop inode/directory
```
# Additional:
## Touche (жесты тачпада):
- `yay -S touchegg && sudo systemctl start touchegg && sudo systemctl enable touchegg`
## DIP
```zsh title="terminal"
yay -S spoofdpi
spoofdpi
brave --proxy-server="http://127.0.0.1:8080"
```
# Wifi connect:
```zsh title="terminal"
iwctl
station list
station wlan0 get-networks
station wlan0 connect <SSID_NAME>
```