# Termux Launcher for GitHub Copilot CLI

A colorful, interactive terminal launcher script designed for use inside a **proot-distro Ubuntu** environment on [Termux](https://termux.dev) on Android. It provides a menu-driven interface to launch [GitHub Copilot CLI](https://docs.github.com/en/copilot/github-copilot-in-the-cli) in your project directories.

> ⚠️ **Important:** This launcher **must** be run inside a `proot-distro login ubuntu` session, **not** directly in Termux. Running it directly in Termux will not work because Termux's sandboxed filesystem does not allow creating top-level directories like `/source`. See [Setup: proot-distro Ubuntu](#setup-proot-distro-ubuntu) below.

![Bash](https://img.shields.io/badge/bash-4%2B-green) ![Platform](https://img.shields.io/badge/platform-proot--distro%20Ubuntu%20%2F%20Linux-blue) ![License](https://img.shields.io/badge/license-MIT-yellow)

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

## Setup: proot-distro Ubuntu

All installation steps below must be run **inside a proot-distro Ubuntu session**. Follow this one-time setup first if you haven't already:

### 1. Install proot-distro and Ubuntu (run in Termux)

```bash
pkg install -y proot-distro
proot-distro install ubuntu
```

### 2. Enter the Ubuntu environment (run in Termux)

```bash
proot-distro login ubuntu
```

> All subsequent commands are run **inside this Ubuntu shell**, not in plain Termux.

## Quick Install (one-liner)

Run this **inside proot-distro login ubuntu** to install everything in one go:

```bash
apt-get install -y git figlet && mkdir -p /source && git clone https://github.com/WillEastbury/termuxlaunchercopilot.git /tmp/tl && cp /tmp/tl/launcher ~/launcher && chmod +x ~/launcher && echo '[ -z "$LAUNCHER_DONE" ] && export LAUNCHER_DONE=1 && exec ~/launcher' >> ~/.bashrc && rm -rf /tmp/tl && echo "Done! Run ~/launcher or open a new terminal session."
```

## Step-by-Step Installation

### Prerequisites

- **Termux** on Android with **proot-distro** installed
- A **proot-distro Ubuntu** environment (see [Setup: proot-distro Ubuntu](#setup-proot-distro-ubuntu) above)
- **GitHub Copilot CLI** — must be installed and authenticated inside Ubuntu (`gh auth login` + `gh extension install github/gh-copilot`)
- **git** — for cloning repos from the launcher

All steps below are run **inside `proot-distro login ubuntu`**, not in plain Termux.

### 1. Install optional dependencies

```bash
# figlet provides the fancy ASCII art banner (optional but recommended)
apt-get install -y figlet
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

To launch the menu automatically every time you enter the Ubuntu proot-distro session, add it to your shell startup file:

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
| `100` | Run bash |
| `101` | Update launcher to the latest version from this repo |

**Navigation:** Use ↑/↓ arrow keys to highlight an option and press Enter, or just type the number.

### Workflow example

1. Open Termux and enter Ubuntu: `proot-distro login ubuntu`
2. The launcher starts automatically (if auto-start is configured)
3. Press `96` to clone a repo (e.g. `myuser/myproject`)
4. Copilot launches in the cloned project directory
5. Next time you enter the Ubuntu session, the project appears as a numbered option

## Upgrading

To upgrade the launcher to the latest version, select option **101** from the menu. The launcher will:

1. Download the latest `launcher` script from this repository
2. Replace your local `~/launcher` with the new version
3. Make it executable and relaunch automatically

Alternatively, you can upgrade manually with this one-liner:

```bash
curl -fsSL https://raw.githubusercontent.com/WillEastbury/termuxlaunchercopilot/main/launcher -o ~/launcher && chmod +x ~/launcher && exec ~/launcher
```

Or using `wget`:

```bash
wget -qO ~/launcher https://raw.githubusercontent.com/WillEastbury/termuxlaunchercopilot/main/launcher && chmod +x ~/launcher && exec ~/launcher
```

## Customisation

The launcher script is a single self-contained Bash file. Edit `~/launcher` to:

- Change the workspace root (default: `/source`) — edit the `SOURCE_ROOT` variable at the top
- Adjust animation speed — tweak the `sleep` values in `matrix_intro`, `swarm_intro`, and `copilot_flyin`
- Change colours — modify the `c1`–`c5` ANSI colour variables

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "Must be run inside proot-distro Ubuntu" error | Run `proot-distro login ubuntu` first, then re-run `~/launcher` |
| No ASCII art banner | Install figlet: `apt-get install -y figlet` |
| Arrows don't work | Make sure you're in an interactive terminal (not piped) |
| Launcher loops on exit | Check the `LAUNCHER_DONE` guard is in your rc file |
| `/source` not found | Run `mkdir -p /source` inside the proot-distro Ubuntu session |

## License

MIT
