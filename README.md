# Ansible Dotfiles Configuration

Ansible playbook for provisioning and managing dotfiles on Fedora (RedHat family). It installs essential packages and keeps application configurations under `~/.config/` in sync via versioned Ansible roles.

## Features

- Installs commonly used packages (terminal, editors, containers, IaC, languages, diagnostics) via `dnf`
- Installs Flatpak applications (Obsidian, Slack, Chrome, VS Code)
- Installs OpenCode globally via npm
- Manages configurations as symlinks to version-controlled role files:
  - Kitty terminal
  - Fastfetch system info
  - Niri compositor
  - Noctalia (config files and settings)
  - Obsidian, VS Code, Zsh
- Auto-imports unmanaged local config files into the roles so everything stays tracked

## Requirements

- Ansible installed on the target host
- Python 3.14 interpreter (`/usr/bin/python3.14`)
- Flatpak and npm available for Flatpak/npm roles

## Installation

Clone the repository:

```bash
git clone git@github.com:Edy-CM/ansible-fedora-config.git
cd ansible-fedora-config
```

## Usage

Run the full playbook (requires sudo, uses `become: true`):

```bash
ansible-playbook site.yml
```

Or run only specific roles by tag:

```bash
ansible-playbook site.yml --tags packages
ansible-playbook site.yml --tags kitty
ansible-playbook site.yml --tags "kitty,fastfetch"
```

### Available tags

| Tag | Description |
| --- | --- |
| `packages` | Installs dnf, Flatpak and npm packages |
| `kitty` | Kitty terminal configuration |
| `fastfetch` | Fastfetch configuration |
| `niri` | Niri compositor configuration |
| `noctalia` | Noctalia configuration |
| `obsidian` | Obsidian configuration |
| `vscode` | Visual Studio Code configuration |
| `zsh` | Zsh configuration |

### Configuration flow

Each config role follows the same pattern:

1. Creates the corresponding directory in `~/.config/<app>/`
2. Imports any unmanaged files already present locally into `roles/<app>/files/`
3. Creates symlinks from `~/.config/<app>/` pointing back to the tracked role files

This keeps a single source of truth in the repository while the live configuration remains usable.

## Project structure

```
.
├── ansible.cfg        # Ansible configuration (inventory, interpreter, host key checking)
├── inventory.yml      # Localhost inventory
├── site.yml           # Main playbook with all roles and tags
├── vault.yml          # Encrypted variables
└── roles/
    ├── packages/      # dnf, Flatpak and npm package installation
    ├── kitty/         # Kitty terminal configuration
    ├── fastfetch/     # Fastfetch configuration
    ├── niri/          # Niri compositor configuration
    ├── noctalia/      # Noctalia configuration
    ├── obsidian/      # Obsidian configuration
    ├── vscode/        # Visual Studio Code configuration
    └── zsh/           # Zsh configuration
```

## License

MIT-0 (see `SPDX-License-Identifier: MIT-0` headers in role files).
