# Termux Launcher for GitHub Copilot CLI

A colorful, interactive terminal launcher script designed for [Termux](https://termux.dev) on Android that provides a menu-driven interface to launch [GitHub Copilot CLI](https://docs.github.com/en/copilot/github-copilot-in-the-cli) in your project directories.

![Bash](https://img.shields.io/badge/bash-4%2B-green) ![Platform](https://img.shields.io/badge/platform-Termux%20%2F%20Linux-blue) ![License](https://img.shields.io/badge/license-MIT-yellow)

## Features

- 🌧️ **Matrix rain intro** — animated startup sequence with colorful glyphs
- 🎨 **Figlet ASCII art banner** — animated slide-in header (falls back gracefully if `figlet` is not installed)
- 🕹️ **Interactive arrow-key menu** — navigate with ↑/↓ and Enter, or type a number directly
- ✈️ **Copilot fly-in animation** — plays when launching Copilot
- 📱 **Responsive layout** — scales to terminal width, works on portrait-oriented mobile screens
- 📂 **Project management:**
  - Browse and open any project directory in Copilot
  - Clone a public GitHub repo and open it in Copilot
  - Create a new project directory and open it in Copilot
  - Open the `/source` workspace directly in Copilot

## Quick Install (one-liner)

Run this in Termux to install everything in one go:

```bash
pkg install -y git figlet && mkdir -p /source && git clone https://github.com/WillEastbury/termuxlaunchercopilot.git /tmp/tl && cp /tmp/tl/launcher ~/launcher && chmod +x ~/launcher && echo '[ -z "$LAUNCHER_DONE" ] && export LAUNCHER_DONE=1 && exec ~/launcher' >> ~/.bashrc && rm -rf /tmp/tl && echo "Done! Run ~/launcher or open a new terminal session."
```

## Step-by-Step Installation

### Prerequisites

- **Termux** (or any Linux terminal with Bash 4+)
- **GitHub Copilot CLI** — must be installed and authenticated (`gh auth login` + `gh extension install github/gh-copilot`)
- **git** — for cloning repos from the launcher

### 1. Install optional dependencies

```bash
# figlet provides the fancy ASCII art banner (optional but recommended)
pkg install -y figlet
```

### 2. Create the source directory

The launcher uses `/source` as the workspace root where your project directories live:

```bash
mkdir -p /source
```

### 3. Download the launcher

```bash
git clone https://github.com/WillEastbury/termuxlaunchercopilot.git /tmp/tl
cp /tmp/tl/launcher ~/launcher
chmod +x ~/launcher
rm -rf /tmp/tl
```

### 4. Auto-start on terminal open (optional)

To launch the menu automatically every time you open a Termux session, add it to your shell startup file:

**For Bash (`~/.bashrc`):**

```bash
echo '[ -z "$LAUNCHER_DONE" ] && export LAUNCHER_DONE=1 && exec ~/launcher' >> ~/.bashrc
```

**For Zsh (`~/.zprofile`):**

```bash
echo '[ -z "$LAUNCHER_DONE" ] && export LAUNCHER_DONE=1 && exec ~/launcher' >> ~/.zprofile
```

> The `LAUNCHER_DONE` guard prevents the launcher from looping if Copilot exits back to the shell.

## Usage

### Run manually

```bash
~/launcher
```

### Menu options

When the launcher starts you'll see an interactive menu listing all directories under `/source`:

| Option | Description |
|--------|-------------|
| `1`–`N` | Open a project directory in Copilot |
| `96` | Clone a public GitHub repo into `/source` and open in Copilot |
| `97` | Open `/source` directly in Copilot |
| `98` | Create a new directory in `/source` and open in Copilot |
| `99` | Exit |

**Navigation:** Use ↑/↓ arrow keys to highlight an option and press Enter, or just type the number.

### Workflow example

1. Open Termux — the launcher starts automatically
2. Press `96` to clone a repo (e.g. `myuser/myproject`)
3. Copilot launches in the cloned project directory
4. Next time you open Termux, the project appears as a numbered option

## Customisation

The launcher script is a single self-contained Bash file. Edit `~/launcher` to:

- Change the workspace root (default: `/source`) — edit the `SOURCE_ROOT` variable at the top
- Adjust animation speed — tweak the `sleep` values in `matrix_intro`, `swarm_intro`, and `copilot_flyin`
- Change colours — modify the `c1`–`c5` ANSI colour variables

## Troubleshooting

| Problem | Fix |
|---------|-----|
| No ASCII art banner | Install figlet: `pkg install figlet` |
| Arrows don't work | Make sure you're in an interactive terminal (not piped) |
| Launcher loops on exit | Check the `LAUNCHER_DONE` guard is in your rc file |
| `/source` not found | Run `mkdir -p /source` |

## License

MIT
