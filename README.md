# Rice Profiles

A Hyprland rice/profile manager for Wayland setups.

This repository contains multiple desktop profiles and a script to quickly switch between them.

## Features

- Hyprland wallpapers
- Waybar themes
- Wofi themes
- Kitty themes
- Starship prompts
- Fastfetch configurations
- One-command profile switching

---

## Repository Structure

```
rice-profiles/
├── luna/
│   ├── fastfetch/
│   │   ├── config.jsonc
│   │   └── image.png
│   ├── kitty/
│   │   └── theme.conf
│   ├── starship/
│   │   └── starship.toml
│   ├── waybar/
│   │   ├── config.jsonc
│   │   └── style.css
│   └── wofi/
│       └── style.css
│
├── melo/
│   ├── fastfetch/
│   │   ├── config.jsonc
│   │   └── image.png
│   ├── kitty/
│   │   └── theme.conf
│   ├── starship/
│   │   └── starship.toml
│   ├── waybar/
│   │   ├── config.jsonc
│   │   └── style.css
│   └── wofi/
│       └── style.css
│
├── wallpapers/
│   ├── luna.jpg
│   └── melo.jpg
│
└── scripts/
    └── profile
```

---

# Installation

## Dependencies

This setup assumes the following software is installed:

- Hyprland
- Waybar
- Wofi
- Kitty
- Starship
- Fastfetch
- Hyprpaper

### Arch / Artix example:

```bash
sudo pacman -S hyprland waybar wofi kitty starship fastfetch hyprpaper
```

---

# Clone the Repository

Clone the repository:

```bash
git clone https://github.com/wmw-frog-enjoyer/rice-profiles.git ~/github/rice-profiles
```

---

# Install the Profile Command

Create the local binary directory if it does not exist:

```bash
mkdir -p ~/.local/bin
```

Create a symlink to the profile script:

```bash
ln -s ~/github/rice-profiles/scripts/profile ~/.local/bin/profile
```

Make sure the script is executable:

```bash
chmod +x ~/github/rice-profiles/scripts/profile
```

Make sure `~/.local/bin` is in your PATH.

Check:

```bash
echo $PATH
```

If missing, add this to your shell configuration:

### Bash

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

### Zsh

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
```

Restart your shell after changing the PATH.

---

# Using Profiles

Available profiles:

```bash
profile luna
```

or:

```bash
profile melo
```

The profile script will automatically:

- Apply Waybar configuration
- Apply Waybar styling
- Apply Wofi styling
- Apply Fastfetch configuration
- Apply Kitty theme
- Apply Starship configuration
- Change wallpaper
- Restart Waybar
- Refresh Fastfetch

---

# How Profiles Work

The system uses symbolic links.

The repository files are the source of truth.

Example:

```
~/.config/waybar/style.css
        |
        v
~/github/rice-profiles/luna/waybar/style.css
```

Editing the file in the repository will directly modify the active configuration.

---

# Creating a New Profile

Create a new profile folder:

```
rice-profiles/
└── my-profile/
    ├── fastfetch/
    │   ├── config.jsonc
    │   └── image.png
    │
    ├── kitty/
    │   └── theme.conf
    │
    ├── starship/
    │   └── starship.toml
    │
    ├── waybar/
    │   ├── config.jsonc
    │   └── style.css
    │
    └── wofi/
        └── style.css
```

Add the wallpaper:

```
wallpapers/my-profile.jpg
```

Then apply it:

```bash
profile my-profile
```

---

# Backing Up Existing Configurations

Before applying profiles on a new machine, it is recommended to backup existing configs:

```bash
mkdir ~/config-backup

cp -r ~/.config/waybar ~/config-backup/
cp -r ~/.config/wofi ~/config-backup/
cp -r ~/.config/kitty ~/config-backup/
cp -r ~/.config/fastfetch ~/config-backup/
cp ~/.config/starship.toml ~/config-backup/
```

---

# Troubleshooting

## Profile command not found

Check:

```bash
which profile
```

Expected output:

```
/home/user/.local/bin/profile
```

If missing:

```bash
ln -s ~/github/rice-profiles/scripts/profile ~/.local/bin/profile
```

---

## Permission denied

Make the script executable:

```bash
chmod +x ~/github/rice-profiles/scripts/profile
```

---

## Waybar does not update

Restart it manually:

```bash
pkill waybar
waybar &
```

---

## Kitty theme does not update

Reload Kitty:

```bash
kitty @ load-config
```

---

# Contributing

Feel free to add new profiles, themes, wallpapers, or improvements.

When adding a profile, keep the same folder structure so the profile script can load it automatically.

---

# License

This project is licensed under the BSD 2-Clause Simplified License.

See [LICENSE](LICENSE) for the full license text.
