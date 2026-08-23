# Rice Profiles

A Hyprland rice/profile manager for Wayland setups.

This repository contains multiple desktop profiles and a script to quickly switch between them.

## Features

* Hyprland wallpapers
* Automatic wallpaper restoration after reboot
* Multi-monitor wallpaper support
* Waybar themes
* Wofi themes
* Kitty themes
* Starship prompts
* Fastfetch configurations
* One-command profile switching
* Symbolic-link based configuration management

---

## Repository Structure

```text
rice-profiles/
├── luna/
│   ├── fastfetch/
│   │   ├── config.jsonc
│   │   └── image.jpg
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
├── scripts/
│   ├── profile
│   └── restore-wallpaper
│
├── state
├── LICENSE
└── README.md
```

---

# Installation

## Dependencies

This setup assumes the following software is installed:

* Hyprland
* Waybar
* Wofi
* Kitty
* Starship
* Fastfetch
* Hyprpaper
* jq

### Arch / Artix example

```bash
sudo pacman -S hyprland waybar wofi kitty starship fastfetch hyprpaper jq
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

Make sure `~/.local/bin` is in your `PATH`.

Check:

```bash
echo $PATH
```

If missing, add this to your shell configuration.

### Bash

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

### Zsh

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
```

Restart your shell after changing the `PATH`.

---

# Configure Wallpaper Restoration

The profile manager remembers the last selected profile and restores its wallpaper when Hyprland starts.

Add the following to your Hyprland configuration:

```ini
exec-once = hyprpaper
exec-once = ~/github/rice-profiles/scripts/restore-wallpaper
```

The `restore-wallpaper` script automatically detects all currently connected monitors and applies the selected profile's wallpaper to each one.

For example, the same configuration works with:

* One laptop display
* A laptop display + external monitor
* Multiple external monitors
* Different monitor configurations

Make sure the restore script is executable:

```bash
chmod +x ~/github/rice-profiles/scripts/restore-wallpaper
```

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

* Apply Waybar configuration
* Apply Waybar styling
* Apply Wofi styling
* Apply Fastfetch configuration
* Apply Kitty theme
* Apply Starship configuration
* Change the wallpaper on all connected monitors
* Save the selected profile
* Restart Waybar
* Refresh Fastfetch

The selected profile is stored in the `state` file so the wallpaper can be restored after restarting Hyprland or rebooting the system.

---

# How Profiles Work

The system uses symbolic links.

The repository files are the source of truth.

For example:

```text
~/.config/waybar/style.css
        |
        v
~/github/rice-profiles/luna/waybar/style.css
```

When the `luna` profile is active, `~/.config/waybar/style.css` points directly to the file inside the repository.

Editing the file in the repository will therefore directly modify the active configuration.

The same approach is used for:

* Waybar
* Wofi
* Fastfetch
* Kitty
* Starship

Wallpapers are not symlinked. Instead, the profile script selects the wallpaper corresponding to the active profile.

---

# Wallpaper Persistence

When a profile is selected:

```bash
profile luna
```

the selected profile name is saved to:

```text
state
```

For example:

```text
luna
```

When Hyprland starts, `restore-wallpaper` reads the saved profile and finds the corresponding wallpaper:

```text
wallpapers/luna.jpg
```

The script then queries Hyprland for all connected monitors and applies the wallpaper to each one.

This means the configuration does not depend on specific monitor names such as `eDP-1` or `HDMI-A-1`.

---

# Creating a New Profile

Create a new profile folder:

```text
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

Add the wallpaper using the same profile name:

```text
wallpapers/my-profile.jpg
```

Then apply it:

```bash
profile my-profile
```

The profile script will automatically detect the new profile.

---

# Backing Up Existing Configurations

Before applying profiles on a new machine, it is recommended to back up existing configurations:

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

```text
/home/user/.local/bin/profile
```

If missing:

```bash
ln -s ~/github/rice-profiles/scripts/profile ~/.local/bin/profile
```

Make sure `~/.local/bin` is in your `PATH`.

---

## Permission denied

Make the scripts executable:

```bash
chmod +x ~/github/rice-profiles/scripts/profile
chmod +x ~/github/rice-profiles/scripts/restore-wallpaper
```

---

## Wallpaper does not appear after reboot

Check that Hyprpaper and the restore script are both configured in `hyprland.conf`:

```ini
exec-once = hyprpaper
exec-once = ~/github/rice-profiles/scripts/restore-wallpaper
```

Check the saved profile:

```bash
cat ~/github/rice-profiles/state
```

It should contain a profile name such as:

```text
luna
```

Also verify that the corresponding wallpaper exists:

```bash
ls ~/github/rice-profiles/wallpapers/
```

---

## Wallpaper does not update

Check that Hyprpaper is running:

```bash
pgrep -a hyprpaper
```

Check that `jq` is installed:

```bash
jq --version
```

You can also manually run the restoration script:

```bash
~/github/rice-profiles/scripts/restore-wallpaper
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

New profiles should also include a wallpaper in:

```text
wallpapers/
```

using the same name as the profile.

For example:

```text
my-profile/
wallpapers/my-profile.jpg
```

---

# License

This project is licensed under the BSD 2-Clause Simplified License.

See [LICENSE](LICENSE) for the full license text.
