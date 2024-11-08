---
tags:
  - Arch
  - Linux
---
# Установка через archinstall:
## Wifi connect:
```zsh title="terminal"
iwctl
station list #Получаем список сетей
station wlan0 get-networks #Находим в списке свою сеть и запоминаем её SSID
station wlan0 connect <SSID_NAME> #Подключаемся к сети подставляя в команду её SSID
```

## Archinstall
Утилита для "автоматической" установки арча
### Запуск
```zsh title="terminal"
archinstall
```
**Для запуска требуется подключение к сети. Если у вас есть возможность поключить проводной интернет - подключайте, если нет то [[#Wifi connect]]**

### Параметры установки
После запуска утилиты перед вами появится консольный интерфейс содержащий опции

**Archinstall language** - Cмена языка интерфейса **установщика, НЕ СИСТЕМЫ** которую вы ставите. Настоятельно **не рекомендую** ее использовать, тк велик шанс что весь текст превратиться в квардратики ▯.

**Locales** - А вот это уже языки ситемы

**Disk сonfiguration** - Разметка диска. Утилита слабая, разделы делает плохо. Лушче всего либо использовать заранее готовую схему разметки, либо дугими утилитами самостоятельно разбить диск, а тут указать точки монтирования.

Готовая схема разметки выглядит так:

| Размер | Файловая cистема | Точка монтирования |
| ------ | ---------------- | ------------------ |
| 1000MB | fat32            | /boot              |
| ост.   | ext4             | /                  |

**Bootloader** - выбор загрузчика. Доступные варианты: **grub**, systemd,.. (моя личная рекомендация - grub)

**Swap** - Создание файла подкачки. При значении True создаст файл подкачки размером 4GB

**Hostname** - Название компьютера.

**User account** - Создание пользователя. При выборе пароля могут появляться ошибки о том что пароль слишком слабый, но это всего лишь предупреждения. Обязательно дайте пользователю права sudo в конце.

**Profile** - Выбор графического окружения и драйверов. (Например Dektop -> gnome). С драйверами лучше всего эксперементировать что лучше пойдет именно на вашем оборудовании.

**Audio** - Выбор звукового сервера (PulseAudio/PipeWire)

**Network сonfiguration** - выбираете: Use Network Manager (иначе инета не быть)

**Timezone** - часовой пояс

**Optional repositories** - выбераем: multilib

**ВСЕ ПУКТЫ КОТОРЫЕ НЕ БЫЛИ ПЕРЕЧИСЛЕНИ ТРОГАЙТЕ НА СВОЙ СТРАХ И РИСК**
# Пакетные менеджеры:
- [[Flatpak]]
- [[Yay]]
- [[Pacman]]

# Post-install:
## Другой формат хранения времени (чтоб не ломалось время при загрузке в винду)
```zsh title="terminal"
sudo timedatectl set-local-rtc 1 --adjust-system-clock
```
## Смена языка на Alt + Shift
```zsh title="terminal"
gsettings set org.gnome.desktop.wm.keybindings switch-input-source "['<Alt>Shift_L']";gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward "['<Shift>Alt_L']"
```

```zsh title="terminal"
gsettings set org.gnome.shell.keybindings show-screenshot-ui "['<Shift><Super>s']";
```
## Отображение восточно-азиатских символов
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
sudo pacman -S steam libreoffice-fresh nvidia-settings flatpak neofetch docker-compose
```
## flatpak:
```zsh title="terminal"
flatpak install flathub org.telegram.desktop com.github.flxzt.rnote com.mattjakeman.ExtensionManager io.github.purplehorrorrus.Meridius md.obsidian.Obsidian com.discordapp.Discord com.dec05eba.gpu_screen_recorder com.github.tenderowl.frog io.github.vikdevelop.SaveDesktop
```
## yay(AUR): 
```zsh title="terminal"
yay -S visual-studio-code-bin brave-bin ttf-droid-sans-mono-dotted-powerline-git docker-desktop
```
# Профили энергопотребления (gnome):
```zsh title="terminal"
sudo pacman -S power-profiles-daemon
```
# Extensions:
- X11 gestures
- Tiling Shell
- Custom Accent Colors
- Caffeine
- blur my shell
- User themes
- Vitals
- Adds AppIndicator / KStatusNotifierItem
- Clipboard Indicator
- Auto Power Profile
- Custom Command Toggle
- Custom Reboot

# Troubleshooting:
## Не включается bluetooth
```zsh title="terminal"
sudo systemctl start bluetooth && sudo systemctl enable bluetooth
```
## VS думает что она nautilus:
```zsh title="terminal"
xdg-mime default org.gnome.Nautilus.desktop inode/directory
```
# Additional:
## Touche (жесты тачпада):
```zsh title="terminal"
yay -S touchegg && sudo systemctl start touchegg && sudo systemctl enable touchegg
```
## DIP
```zsh title="terminal"
yay -S spoofdpi
spoofdpi
brave --proxy-server="http://127.0.0.1:8080"
```

## Run comand from admin with pass modal
```zsh title="terminal"
pkexec /usr/bin/env DISPLAY=$DISPLAY XAUTHORITY=$XAUTHORITY <comand>
```

## Game mode run
```zsh title="terminal"
sudo pacman -S gamemode
```
В стиме добавить параметры запуска к игре:
- `gamemoderun %command%`

## Hydra Launcher
- Install from https://github.com/hydralauncher/hydra/releases
- add sources from https://hydralinks.cloud/