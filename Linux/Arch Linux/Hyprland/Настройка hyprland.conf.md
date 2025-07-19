Самый главный файл настройки **hypland**

```bash
nano .config/hypr/hyprland.conf
```

[[#1. Monitors]]
[[#2. My Programs]]
[[#3. Autostart]]
[[#4. Environment Variables]]
[[#6. Look and Feel]]
[[#7. Input]]
[[#8. Keybindings]]
[[#9. Windows and Workspaces]]

---

# 1. Monitors

Для начала нужно понять, какой монитор у нас 

```bash
hyprctl monitors
```

Забираем свои значения где:

- `eDP-1` - это имя монитора (output name). Обычно это имя, которое система присваивает дисплею.
- `2560x1600@165.01900` - это настройки разрешения и частоты обновления.
- `0x0` - это позиция монитора в пространстве рабочих столов (координаты верхнего левого угла)
- `1.6` - это масштабирование (scale factor).

```
monitor=eDP-1,2560x1600@165.01900,0x0, 1.6
```

---
# 2. My Programs

```bash
# Программы присвоенные переменным

$terminal = kitty

$fileManager = nautilus

$menu = 

$firefox = firefox

$obsidian = obsidian --force-device-scale-factor=1.5

$vscode = code --force-device-scale-factor=1.5

$telegram = Telegram --force-device-scale-factor=1.5
```

Выравнивает масштаб приложения `--force-device-scale-factor=1.5`

---

# 3. Autostart

Здесь мы подключаем те программы/службы которые будут автоматический при входе в систему включаться

```
exec-once = hyprpaper
```

В конфигурации Hyprland команда `exec-once` используется для **однократного запуска приложений/скриптов при старте оконного композитора**.

---
# 4. Environment Variables

Этот раздел в конфигурации Hyprland отвечает за **настройку переменных окружения**.

```hyprland
env = XCURSOR_SIZE,24      # Устанавливает размер системного курсора в пикселях
env = HYPRCURSOR_SIZE,24   # Специфичная для Hyprland настройка размера курсора
```

Этот блок:

1. Задаёт **глобальные переменные окружения** для всей сессии Hyprland

2. Влияет на поведение приложений, запущенных в Wayland-среде

3. Обеспечивает единообразие интерфейса

---

# 5. Permissions

[[#Основное назначение раздела]]
[[#Ключевые элементы]]
[[#Примеры из конфига]]
[[#Почему это важно?]]
[[#Как использовать?]]
[[#Распространённые типы действий]]
[[#Важные предупреждения]]

Этот раздел конфигурации Hyprland отвечает за **управление разрешениями безопасности** для приложений. Он контролирует, какие программы могут выполнять определённые привилегированные действия в системе.

### Основное назначение раздела

Это система безопасности, которая:

1. Запрещает приложениям выполнять чувствительные операции без явного разрешения

2. Защищает от потенциально опасных действий (например, несанкционированный захват экрана)

3. Особенно важна при использовании Wayland

### Ключевые элементы

1. `ecosystem` блок (закомментирован):

```hyprland
# ecosystem {
#   enforce_permissions = 1
# }
```


- Включение основного механизма контроля разрешений

- Если раскомментировать - **система разрешений станет активной**

- По умолчанию отключена для совместимости


2. **Правила разрешений**:

```hyprland
# permission = /путь/к/программе, действие, разрешение
```

- **Первый параметр**: Путь к приложению (можно использовать регулярные выражения)

- **Второй параметр**: Тип действия

- **Третий параметр**: `allow` (разрешить) или `deny` (запретить)

### Примеры из конфига

1. Для утилиты скриншотов `grim`:

```hyprland
# permission = /usr/(bin|local/bin)/grim, screencopy, allow
```

- Разрешает `grim` делать скриншоты (`screencopy`)

2. Для портала рабочего стола:

```hyprland
# permission = /usr/(lib|libexec|lib64)/xdg-desktop-portal-hyprland, screencopy, allow
```

- Разрешает системному порталу доступ к функции захвата экрана

3. Для менеджера плагинов:

```hyprland
# permission = /usr/(bin|local/bin)/hyprpm, plugin, allow
```

- Разрешает `hyprpm` управлять плагинами Hyprland

### Почему это важно?

1. **Безопасность**: Предотвращает:
    
    - Несанкционированный доступ к камере/микрофону
        
    - Тайный захват экрана
        
    - Чтение ввода с клавиатуры
        
2. **Конфиденциальность**: Защищает от:
    
    - Шпионских программ
        
    - Кейлоггеров
        
    - Нежелательного скриншотирования
        
3. **Стабильность**: Блокирует:
    
    - Неавторизованное изменение плагинов
        
    - Системные модификации

### Как использовать?

1. Раскомментируйте блок `ecosystem`

2. Добавьте разрешения для нужных программ:

```hyprland
ecosystem {
  enforce_permissions = 1
}

permission = /usr/bin/wf-recorder, screencopy, allow
permission = /usr/bin/slurp, screencopy, allow

```

3. После изменения **требуется полный перезапуск Hyprland** (не релоад!)

### Распространённые типы действий:

|Тип разрешения|Для чего используется|
|---|---|
|`screencopy`|Захват экрана/скриншоты|
|`idleinhibit`|Предотвращение перехода в сон|
|`global_shortcuts`|Глобальные хоткеи|
|`plugin`|Управление плагинами Hyprland|
### Важные предупреждения:

1. Без настроенных разрешений многие приложения **не будут работать**

2. Все изменения требуют **полного перезапуска сессии**

3. При ошибках в конфиге Hyprland может отказаться запускаться

4. Для диагностики используйте:

```bash
journalctl -u hyprland
```

Если вы не используете строгие ограничения безопасности, можно оставить этот раздел закомментированным.

---
# 6. Look and Feel

Этот раздел `general` в конфигурации Hyprland отвечает за **основные визуальные настройки и поведение окон**. Давайте подробно разберём каждое свойство:

[[#general]]
[[#decoration]]
[[#dwidle]]
[[#master]]
[[#misc]]
### general

```hyprland
general {
	gaps_in = 5
	gaps_out = 20
	border_size = 2

	
	col.active_border = rgba(b4befeFF)
	col.inactive_border = rgba(6c7298FF)

	resize_on_border = false
	allow_tearing = false
	layout = dwindle
}
```

`gaps_in = 5`  Внутренние отступы между окнами на одном рабочем столе. Создает воздушное пространство между соседними окнами.
`gaps_out = 20` Внешние отступы от краёв экрана. Оставляет свободное пространство между окнами и краями монитора
`border_size = 2` Толщина рамки окон. Обычно 1-5px, для фокусного режима можно установить 0

 `col.active_border = rgba(b4befeFF)` Цвет рамки активного (фокусированного) окна
 `col.inactive_border = rgba(6c7298FF)` Цвет рамки неактивных окон

 `resize_on_border = false` Разрешить изменение размера окна при наведении на границу. Если `true`, можно менять размер без комбинации клавиш
 `allow_tearing = false` Разрешить разрывы изображения (для уменьшения задержки в играх). Включайте только для игр, может вызывать артефакты
 `layout = dwindle` Раскладка окон по умолчанию
- **Варианты**:
    
    - `dwindle`: Стековая раскладка (как в i3)
    
    - `master`: Главное окно + второстепенные (как в dwm)
    
    - `none`: Без автоматической раскладки


### decoration

```hyprland
decoration {
	rounding = 10
	rounding_power = 2
	active_opacity = 1.0
	inactive_opacity = 1.0
    
    shadow {

		enabled = true
		range = 4
		render_power = 3
		color = rgba(1a1a1aee)
	}

	blur {

		enabled = true
		size = 3
		passes = 1
		vibrancy = 0.1696
	}
}
```

Основные параметры:

```hyprland
rounding = 10               # Радиус скругления углов окон (в пикселях)
rounding_power = 2          # Качество рендеринга скруглений (1-3, где 3 - наивысшее)
active_opacity = 1.0        # Непрозрачность активного окна (1.0 = 100%)
inactive_opacity = 1.0      # Непрозрачность неактивных окон
```

 Блок `shadow` (тени):

```hyprland
shadow {
    enabled = true          # Включение теней
    range = 4               # Размер/радиус тени (в пикселях)
    render_power = 3        # Качество рендеринга теней (чем выше, тем лучше качество и нагрузка)
    color = rgba(1a1a1aee)  # Цвет тени в формате RGBA (#1a1a1a с прозрачностью 0xEE)
}
```

- **Тени** отбрасываются от всех окон

- `range=4` - мягкая неагрессивная тень

- `color` - тёмно-серый цвет с высокой непрозрачностью (93%)

Блок `blur` (размытие):

```hyprland
blur {
    enabled = true          # Включение эффекта размытия
    size = 3                # Интенсивность размытия (радиус в пикселях)
    passes = 1              # Количество проходов размытия (качество)
    vibrancy = 0.1696       # Усиление насыщенности цветов после размытия
}
```

- **Размытие** применяется к:
    
    - Полупрозрачным элементам
    
    - Обоям под прозрачными окнами
    
    - Элементам интерфейса
    
- `vibrancy` - компенсирует потерю насыщенности при размытии

### dwidle

Раздел `dwindle` в конфигурации Hyprland отвечает за настройки **тайлинговой (плиточной) раскладки окон**. Dwindle - это один из двух основных режимов компоновки окон в Hyprland (наряду с `master`).

```
dwindle {
	pseudotile = true 
	preserve_split = true
}
```

 `pseudotile = true` Псевдо-тайлинг (гибридный режим). Окна ведут себя как тайловые (автоматически размещаются без перекрытий). Но при ручном перемещении/изменении размера временно становятся "плавающими". После отпускания мыши возвращаются в тайловую компоновку

 `preserve_split = true` Сохранение структуры разделения экрана. При закрытии окна, оставшиеся окна сохраняют свои пропорции и расположение. Новые окна открываются в существующих "слотах" **Поведение при `false`** Закрытие окна приводит к полной перекомпоновке рабочей области. Новые окна занимают всё доступное пространство

### master

Раздел `master` в конфигурации Hyprland относится к настройкам **альтернативной тайлинговой раскладки окон**, вдохновленной оконным менеджером dwm (Dynamic Window Manager).

 **Что такое раскладка "master"?**

Это двухпанельная система управления окнами, где:

1. **Главное окно (master)** - занимает основную часть экрана

2. **Вторичные окна (stack)** - располагаются вертикально/горизонтально в оставшейся области

```
master {
    new_status = master
}
```

**Значение параметра:**

- **`new_status = master`**  
    Определяет поведение **новых окон** при создании:
    
    - `master`: Новое окно автоматически становится **главным** окном
    
    - `stack`: Новое окно открывается как **вторичное**
    
    - `last`: Сохраняет статус предыдущего окна

```
┌───────────────────────┐
│                       │
│      MASTER (1)       │
│                       │
└───────────────────────┘
```

```
┌───────────┬───────────┐
│           │           │
│  MASTER   │   STACK   │
│   (2)     │    (1)    │
└───────────┴───────────┘
```

```
┌───────────┬───────────┐
│           │   STACK   │
│  MASTER   ├───────────┤
│   (3)     │   STACK   │
│           │    (1)    │
└───────────┴───────────┘
```

```
master {
    new_status = master
    mfact = 0.55                 # Доля экрана для мастер-окна (55%)
    orientation = left           # Расположение стека: left/right/top/bottom
    always_center_master = true  # Центрировать мастер-окно
    special_scale_factor = 0.8   # Масштаб для special workspace
    no_gaps_when_only = true     # Убирать отступы при одном окне
}
```

### misc

Раздел `misc` (miscellaneous) в конфигурации Hyprland содержит **разные дополнительные настройки**, которые не вошли в другие категории.

`force_default_wallpaper = -1` 

`-1`: **Отключить** обои по умолчанию (рекомендуется при использовании hyprpaper/swww
 `0`: Использовать **первые** встроенные обои
 `1`: Использовать **вторые** встроенные обои и т.д.

**Практика**: Всегда `-1` если вы используете свои обои

`disable_hyprland_logo = false` Управляет логотипом Hyprland в углу экрана. 
`false`: **Показывать** логотип (по умолчанию)
`true`: **Скрыть** логотип

Для чистого рабочего стола установите `true`

```hyprland
misc {
    # Основные
    vfr = true    # Вертикальная синхронизация (VSync)
    vrr = 1       # Переменная частота обновления (0-выкл, 1-вкл, 2-полный экран)
    
    # Поведение
    animate_manual_resizes = true # Анимация изменения размера окон
    enable_swallow = true       # "Проглатывание" окон (например, терминал запускает GUI)
    swallow_regex = ^(kitty)$   # Шаблон для "проглатывания" (например, все окна kitty)
    
    # Производительность
    disable_splash_rendering = false # Отключить стартовую анимацию
    force_hypr_chan = false     # Экспериментальный рендеринг (только для разработчиков)
    
    # Интерфейс
    mouse_move_enables_dpms = true # Включение экрана при движении мыши
    key_press_enables_dpms = true  # Включение экрана при нажатии клавиш
}
```

Рекомендуемая конфигурация для большинства пользователей:

```hyprland
misc {
    force_default_wallpaper = -1
    disable_hyprland_logo = true    # Скрыть лого
    vrr = 1                         # Для игр с поддержкой Freesync/G-Sync
    enable_swallow = true           # Полезно для терминальных запусков
    mouse_move_enables_dpms = true  # Автовключение экрана
    disable_splash_rendering = true # Ускорить запуск
}
```

---
# 7. Input

```hyprland
input {
    # Основные настройки клавиатуры
    kb_layout = us,ru        # Раскладки клавиатуры: английская и русская
    kb_variant =             # Вариант раскладки (например, dvorak, macintosh)
    kb_model =               # Специфичная модель клавиатуры (обычно не требуется)
    kb_options = grp:win_space_toggle  # Переключение раскладки Win+Space
    kb_rules =               # Правила XKB (для сложных кастомизаций)

    # Поведение мыши
    follow_mouse = 1         # Фокус следует за курсором (1-включено)
    sensitivity = 0          # Коррекция чувствительности мыши (-1.0 до 1.0)

    # Настройки тачпада
    touchpad {
        natural_scroll = false  # Обратная прокрутка (true-как в macOS)
    }
}

# Жесты
gestures {
    workspace_swipe = false  # Переключение рабочих столов свайпом (false-отключено)
}

# Настройки для конкретного устройства
device {
    name = epic-mouse-v1     # Имя устройства из `hyprctl devices`
    sensitivity = -0.5       # Индивидуальная чувствительность мыши (-0.5 = снижение на 50%)
}
```

---
# 8. Keybindings 

Горячие клавиши

```hyprland
# Основная модификаторная клавиша (SUPER = Win/Command)
$mainMod = SUPER

# Основные команды
bind = $mainMod, RETURN, exec, $terminal  # Открыть терминал
bind = $mainMod, Q, killactive,           # Закрыть активное окно
bind = $mainMod, M, exit,                 # Завершить сессию Hyprland (выход)
bind = $mainMod, E, exec, $fileManager    # Открыть файловый менеджер
bind = $mainMod, F, exec, $firefox        # Открыть браузер Firefox
bind = $mainMod, O, exec, $obsidian       # Запустить Obsidian (для заметок)
bind = $mainMod, C, exec, $vscode         # Запустить Visual Studio Code
bind = $mainMod, T, exec, $telegram       # Запустить Telegram

# Управление окнами
bind = $mainMod, V, togglefloating,       # Переключить окно в плавающий режим
bind = $mainMod, R, exec, $menu           # Открыть меню запуска приложений

# Управление тайлингом (для раскладки dwindle)
bind = $mainMod, P, pseudo,               # Вкл/выкл псевдо-тайлинг
bind = $mainMod, J, togglesplit,          # Переключить ориентацию разделения
```

```hyprland
# Перемещение фокуса между окнами с помощью mainMod + стрелок
bind = $mainMod, left, movefocus, l   # Фокус на окно слева
bind = $mainMod, right, movefocus, r  # Фокус на окно справа
bind = $mainMod, up, movefocus, u     # Фокус на окно выше
bind = $mainMod, down, movefocus, d   # Фокус на окно ниже

# Переключение между рабочими столами с помощью mainMod + цифра
bind = $mainMod, 1, workspace, 1     # Перейти на рабочий стол 1
bind = $mainMod, 2, workspace, 2     # Перейти на рабочий стол 2
bind = $mainMod, 3, workspace, 3     # Перейти на рабочий стол 3
bind = $mainMod, 4, workspace, 4     # Перейти на рабочий стол 4
bind = $mainMod, 5, workspace, 5     # Перейти на рабочий стол 5
bind = $mainMod, 6, workspace, 6     # Перейти на рабочий стол 6
bind = $mainMod, 7, workspace, 7     # Перейти на рабочий стол 7
bind = $mainMod, 8, workspace, 8     # Перейти на рабочий стол 8
bind = $mainMod, 9, workspace, 9     # Перейти на рабочий стол 9
bind = $mainMod, 0, workspace, 10    # Перейти на рабочий стол 10

# Перемещение активного окна на другой рабочий стол с помощью mainMod + SHIFT + цифра
bind = $mainMod SHIFT, 1, movetoworkspace, 1   # Переместить окно на рабочий стол 1
bind = $mainMod SHIFT, 2, movetoworkspace, 2   # Переместить окно на рабочий стол 2
bind = $mainMod SHIFT, 3, movetoworkspace, 3   # Переместить окно на рабочий стол 3
bind = $mainMod SHIFT, 4, movetoworkspace, 4   # Переместить окно на рабочий стол 4
bind = $mainMod SHIFT, 5, movetoworkspace, 5   # Переместить окно на рабочий стол 5
bind = $mainMod SHIFT, 6, movetoworkspace, 6   # Переместить окно на рабочий стол 6
bind = $mainMod SHIFT, 7, movetoworkspace, 7   # Переместить окно на рабочий стол 7
bind = $mainMod SHIFT, 8, movetoworkspace, 8   # Переместить окно на рабочий стол 8
bind = $mainMod SHIFT, 9, movetoworkspace, 9   # Переместить окно на рабочий стол 9
bind = $mainMod SHIFT, 0, movetoworkspace, 10  # Переместить окно на рабочий стол 10
```

```hyprland
# Специальный рабочий стол (scratchpad) - временная рабочая область
bind = $mainMod, S, togglespecialworkspace, magic          # Переключить специальный рабочий стол "magic"
bind = $mainMod SHIFT, S, movetoworkspace, special:magic   # Переместить активное окно на специальный рабочий стол "magic"

# Листание рабочих столов с помощью колеса мыши
bind = $mainMod, mouse_down, workspace, e+1  # Перейти к следующему рабочему столу (прокрутка вниз)
bind = $mainMod, mouse_up, workspace, e-1    # Перейти к предыдущему рабочему столу (прокрутка вверх)

# Управление окнами с помощью мыши
bindm = $mainMod, mouse:272, movewindow     # Перетаскивание окон: зажать mainMod + ЛКМ
bindm = $mainMod, mouse:273, resizewindow   # Изменение размера: зажать mainMod + ПКМ

# Управление громкостью
bindel = ,XF86AudioRaiseVolume, exec, wpctl set-volume -l 1 @DEFAULT_AUDIO_SINK@ 5%+  # Увеличить громкость (+5%, макс. 100%)
bindel = ,XF86AudioLowerVolume, exec, wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-        # Уменьшить громкость (-5%)
bindel = ,XF86AudioMute, exec, wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle              # Вкл/выкл звук
bindel = ,XF86AudioMicMute, exec, wpctl set-mute @DEFAULT_AUDIO_SOURCE@ toggle         # Вкл/выкл микрофон

# Управление яркостью экрана
bindel = ,XF86MonBrightnessUp, exec, brightnessctl -e4 -n2 set 5%+   # Увеличить яркость (+5%)
bindel = ,XF86MonBrightnessDown, exec, brightnessctl -e4 -n2 set 5%- # Уменьшить яркость (-5%)

# Управление медиаплеером (требует установленного playerctl)
bindl = , XF86AudioNext, exec, playerctl next          # Следующий трек
bindl = , XF86AudioPause, exec, playerctl play-pause   # Пауза/продолжить
bindl = , XF86AudioPlay, exec, playerctl play-pause    # Пауза/продолжить (дублирующая клавиша)
bindl = , XF86AudioPrev, exec, playerctl previous      # Предыдущий трек
```

---
# 9. Windows and Workspaces

```hyprland
# Основные правила для окон
windowrule = suppressevent maximize, class:.*  # Игнорировать запросы на максимизацию от всех приложений

# Исправление проблем с фокусом для XWayland-окон
windowrule = nofocus,class:^$,title:^$,xwayland:1,floating:1,fullscreen:0,pinned:0

# Настройки XWayland
xwayland {
 force_zero_scaling = true  # Форсировать масштабирование 100% для XWayland-приложений
}

# Специальные правила для файлового менеджера Nautilus
windowrulev2 = float, class:^(org.gnome.Nautilus)$         # Открывать в плавающем режиме
windowrulev2 = center, class:^(org.gnome.Nautilus)$        # Позиционировать по центру экрана
windowrulev2 = size 1000 600, class:^(org.gnome.Nautilus)$ # Установить размер 1000x600 пикселей
windowrulev2 = pin, class:^(org.gnome.Nautilus)$           # Открывать поверх других окон
```

---

[[Arch Linux Database]]