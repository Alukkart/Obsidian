---
tags:
  - Arch
  - Linux
---
# Установка через archinstall:

# Post-install:
- `sudo timedatectl set-local-rtc 1 --adjust-system-clock` - Другой формат хранения времени (чтоб не ломалось время при загрузке в винду)
- `gsettings set org.gnome.desktop.wm.keybindings switch-input-source "['<Alt>Shift_L']";gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward "['<Shift>Alt_L']"` - Смена языка на Alt + Shift
- `pacman -S ttf-sazanami` - Отображение восточно азиатских символов
- `sudo pacman -S --needed base-devel git && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si` - Установка Yay(AUR)
# Основные программы:
- ОФ репозитории - `sudo pacman -S steam libreoffice-fresh nvidia-settings neofetch`
- flatpak - `flatpak install flathub org.telegram.desktop com.github.flxzt.rnote com.mattjakeman.ExtensionManager io.github.purplehorrorrus.Meridius md.obsidian.Obsidian com.discordapp.Discord com.dec05eba.gpu_screen_recorder com.github.tenderowl.frog flatpak install flathub io.github.vikdevelop.SaveDesktop`
- yay(AUR) - `yay -S visual-studio-code-bin brave-bin ttf-droid-sans-mono-dotted-powerline-git`
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
- VS думает что она nautilus - `xdg-mime default org.gnome.Nautilus.desktop inode/directory`
# Additional:
## OhMyZsh:
- Установка - `sudo pacman -S zsh && sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
- Темы - ~/.zshrc `ZSH_THEME="robbyrussell"`=> `ZSH_THEME="agnoster"` 
- Шрифты - `git clone https://github.com/powerline/fonts.git && cd fonts && ./install.sh && cd ../ && rm -rf fonts`
## Touche (жесты тачпада):
- `sudo dnf install touchegg && sudo systemctl start touchegg && sudo systemctl enable touchegg`
# Wifi connect:
- `iwctl`
- `station list`
- `station wlan0 get-networks`
- `station wlan0 connect <SSID_NAME>`

# DIP
- `spoof-dpi`
- `brave --proxy-server="http://127.0.0.1:8080"`
