## Очистка кеша:
```bash
yay -Scc
```

## Паралельная сборка (больше ядер)
```zsh
sudo nano /etc/makepkg.conf
```

```conf
# Использовать все ядра (-j$(nproc) или конкретное число)
MAKEFLAGS="-j$(nproc)"
# Все ядра -4 т.е если у процессора 8 ядер, то будет использовано только 6
MAKEFLAGS="-j$(($(nproc) - 2))"
# Или вручную (например, для 8 ядер)
MAKEFLAGS="-j8"

# Распараллелить сжатие (если поддерживается)
COMPRESSXZ=(xz -c -z - --threads=0)  # 0 = все потоки
COMPRESSZST=(zstd -c -z -q - --threads=0)
```
