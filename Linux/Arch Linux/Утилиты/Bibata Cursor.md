**Bibita Cursor для Arch Linux**

Можно поставить разным способом. Самый простой и универсальный, это терминал.

```bash
yay -S bibata-cursor-theme-bin
```

После установки, проверяем установленный файл

```bash
ls /usr/share/icons/Bibata-Modern-Ice
```

В Gnome устанавливаем через gnome-tweaks, а вот для Hyprland 

Переходим в папку `.config/hypr` и открываем файл `hyprland.conf`, добавляем в загрузки

```hyprland
exec = hyprctl setcursor Bibata-Modern-Ice 20
```

