# Termux Launcher for GitHub Copilot CLI

A colorful, interactive terminal launcher script designed for Termux (Android) that provides a menu-driven interface to launch GitHub Copilot CLI in your project directories.

## Features

- **Matrix rain intro** — animated startup sequence with colorful glyphs
- **Figlet ASCII art banner** — animated slide-in "Swarmy" header (falls back gracefully if `figlet` is not installed)
- **Interactive arrow-key menu** — navigate with ↑/↓ and Enter, or type a number
- **Copilot fly-in animation** — plays when launching Copilot
- **Responsive layout** — scales to terminal width, works on portrait-oriented screens
- **Built-in actions:**
  - Open any project directory in Copilot
  - Clone a public repo and open in Copilot
  - Create a new directory and open in Copilot
  - Open `/source` directly in Copilot

## Requirements

- Bash 4+
- [GitHub Copilot CLI](https://githubnext.com/projects/copilot-cli)
- Optional: `figlet` (for ASCII art banner)

## Installation

```bash
# Clone the repo
git clone https://github.com/WillEastbury/termuxlaunchercopilot.git

# Copy the launcher to your home directory
cp termuxlaunchercopilot/launcher ~/launcher
chmod +x ~/launcher

# Optionally add to your shell startup (~/.zprofile for zsh)
echo '[ -z "$LAUNCHER_DONE" ] && export LAUNCHER_DONE=1 && exec ~/launcher' >> ~/.zprofile
```

## Usage

```bash
~/launcher
```

## License

MIT
