- Установить [ZSH](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)
- Установить ohMyZsh`sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
- Установить тему powerlevel10k `git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`
- Выбрать тему - ~/.zshrc `ZSH_THEME="robbyrussell"`=> `ZSH_THEME="powerlevel10k/powerlevel10k"` 
- Скачать [Шрифты](https://github.com/romkatv/powerlevel10k/blob/master/font.md)

## Плагины
- Autosuggestions - `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
- Подсветка синтаксиса - `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

## Включение плагинов
добавить в .zshrc
```
plugins=( 
    # other plugins...
    zsh-autosuggestions
    zsh-syntax-highlighting
)
```