
```bash
sudo pacman -S waybar
```

Создаем папку 

```bash
cd .config && mkdir waybar
```

Создаем в папке waybar два файла

```bash
touch config style.css
```

Скачиваю пакеты для отоброжения иконок

```bash
sudo pacman -S ttf-fira-code
sudo pacman -S ttf-font-awesome
sudo pacman -S ttf-nerd-fonts-symbols
```

## Config

```config
{
	"layer": "top",
	"position": "top",
	"height": 30,
	"spacing": 4,
	"modules-left": [
		"custom/archicon",
		"hyprland/workspaces"
	],
	"modules-center": [
		"custom/mpv-track"
	],
	"modules-right": [],
	"custom/archicon": {
		"format": "󰣇",
		"tooltip": false,
		"on-click": "nautilus"
	},
	"hyprland/workspaces": {
		"format": "●",
		"persistent-workspaces": {
			"*": 7
		}
	}
}
```


### style.css

```css
* {
font-family: 'JetBrainsMono Nerd Font', 'Symbols Nerd Font';
font-weight: bold;
font-size: 13.2px; /* -15% */
min-height: 0px;
border: none;
border-radius: 0px;
}

window#waybar {
background: transparent;
}

.modules-left {
margin-left: 20px; /* -15% */
margin-top: 10px;
}

.modules-center {
margin-top: 10px;
}

.modules-right {
margin-right: 17px; /* -15% */
background: #181825;
border-radius: 6.8px; /* -15% */
margin-top: 10px;
}

#custom-archicon {
color: #b4befe; /* Основной цвет */
border-right: 1px solid;
border-image: linear-gradient(
45deg,
#b4befe 0%,
#a5b0ff 50%,
#96a2ff 100%,
#96a2ff 100%
)
1;
border-radius: 8px;
padding: 3px 15px 0 0;
margin: 4px 10px 4px 0;
font-family: 'JetBrains Mono';
font-weight: bold;
font-size: 28px;
}

#workspaces {
padding: 0px 3px;
margin-right: 30px;
}

#workspaces button {
all: unset;
padding: 0px 3px;
color: rgba(180, 190, 254, 0.4);
transition: all 0.2s ease;
}

#workspaces button:hover {
color: rgba(0, 0, 0, 0);
border: none;
text-shadow: 0px 0px 1.5px rgba(180, 190, 254, 0.7);
transition: all 1s ease;

}

#workspaces button.active {
color: #b4befe;
border: none;
}
```


