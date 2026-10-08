# Super ISO Updater (fork)

A fork of [JoshuaVandaele/SuperISOUpdater](https://github.com/JoshuaVandaele/SuperISOUpdater) with our own set of updaters and fixes. It checks for updates and installs the latest versions of various ISO files, designed to work with a Ventoy drive.

This fork keeps the upstream project as a remote and pulls in its changes selectively. Our `main` branch carries our customizations; upstream work is merged in when it's useful rather than tracked continuously.

## What's different in this fork

- **11 additional Linux distro updaters**: Ventoy, MX Linux, Puppy Linux, Parrot Security, Pop!_OS, EndeavourOS, Xubuntu, Kubuntu, Lubuntu, Ubuntu MATE, and Ubuntu Budgie.
- **Duplicate-download fix**: when a folder holds multiple versions of an ISO, the updater now picks the highest version instead of the first file alphabetically, so it stops re-downloading ISOs you already have.
- **Version parsing fix**: version strings with repeated separators (e.g. `6...4`) no longer crash version comparison.
- **Windows file-lock retry**: `.part` renames retry three times with a one-second gap when an antivirus or indexer briefly locks the file.
- **Proxmox fix**: version detection updated for the redesigned download page, and the `arm64` variant is excluded from version selection.
- **Windows 11 language support**: uses the dynamic `[[LANG]]` placeholder instead of a hardcoded language.
- **OPNsense re-categorized** under a new `OperatingSystems.Network` section (the old location still works).
- **CI**: a `build-exe.yml` workflow builds `sisou.exe` with PyInstaller and attaches it to each release.

## Getting Started

### Prerequisites

- Python 3.12 installed on your system.

### Installation

#### Using pip

```sh
python -m pip install sisou
```

#### Using git

```sh
git clone https://github.com/jaydenthorup/SuperISOUpdater
cd SuperISOUpdater
python -m pip install .
```

### Updating

```sh
python -m pip install --upgrade sisou
```

## Usage

```sh
sisou <Ventoy Partition>
```

#### Example on Windows

```sh
sisou E:
```

#### Example on Linux

```sh
sisou /run/media/joshua/Ventoy/
```

### Logging

Control the log level with `-l` / `--log-level` (DEBUG, INFO, WARNING, ERROR, CRITICAL; default INFO):

```sh
sisou <Ventoy Partition> -l DEBUG
```

Save logs to a file with `-f` / `--log-file`:

```sh
sisou <Ventoy Partition> -f /path/to/log_file.log
```

## Customization

The script uses a `config.toml` file to define which ISOs to update. Each ISO maps to an updater class (e.g. `Ubuntu`, `MemTest86Plus`); enable or disable ISOs by editing the relevant sections.

_NOTE: Be cautious when modifying the configuration file, as incorrect changes may cause the script to malfunction._

By default the script looks for `config.toml` in the same directory as the Ventoy drive. Specify a custom file with `-c` / `--config-file`:

```sh
sisou <Ventoy Partition> -c /path/to/config.toml
```

## Supported ISOs

- **Diagnostic Tools**
  - Hiren's BootCD PE
  - MemTest86 Plus
  - SystemRescue
  - UltimateBootCD
  - Rescuezilla (editions: "bionic", "focal", "jammy", "noble")
- **Boot Repair**
  - Super Grub 2
- **Disk Utilities**
  - Clonezilla
  - GParted Live
  - ShredOS
  - HDAT2 (editions: "full", "lite", "diskette")
- **Operating Systems**
  - **Linux**
    - Arch Linux
    - Debian (editions: "standard", "cinnamon", "kde", "gnome", "lxde", "lxqt", "mate", "xfce")
    - Ubuntu (editions: "LTS", "interim")
    - Fedora (editions: "Budgie", "Cinnamon", "KDE", "LXDE", "MATE_Compiz", "SoaS", "Sway", "Xfce", "i3")
    - Kali Linux (editions: "installer", "installer-netinst", "installer-purple", "live")
    - Linux Mint (editions: "cinnamon", "mate", "xfce")
    - Manjaro (editions: "plasma", "xfce", "gnome", "cinnamon", "i3")
    - OpenSUSE (editions: "leap", "leap-micro", "jump")
    - OpenSUSE Rolling (editions: "MicroOS-DVD", "Tumbleweed-DVD", "Tumbleweed-NET", "Tumbleweed-GNOME-Live", "Tumbleweed-KDE-Live", "Tumbleweed-XFCE-Live", "Tumbleweed-Rescue-CD")
    - Proxmox (editions: "ve", "mail-gateway", "backup-server")
    - Rocky Linux (editions: "dvd", "boot", "minimal")
    - Tails
    - ChromeOS (editions: "ltc", "ltr", "stable")
    - **Ventoy**
    - **MX Linux**
    - **Puppy Linux**
    - **Parrot Security**
    - **Pop!_OS**
    - **EndeavourOS**
    - **Xubuntu**
    - **Kubuntu**
    - **Lubuntu**
    - **Ubuntu MATE**
    - **Ubuntu Budgie**
  - **Windows**
    - Windows 11 (Multi-edition ISO, Any language)
    - Windows 10 (Multi-edition ISO, Any language)
  - **BSD**
    - TrueNAS (editions: "scale", "core")
  - **Network**
    - OPNsense (editions: "dvd", "nano", "serial", "vga")
  - **Other**
    - FreeDOS (editions: "BonusCD", "FloppyEdition", "FullUSB", "LegacyCD", "LiteUSB", "LiveCD")
    - TempleOS (editions: "Distro", "Lite")

## Keeping up with upstream

This fork tracks upstream as a remote. To pull in upstream changes:

```sh
git fetch upstream
git merge upstream/main
```

Resolve any conflicts as needed. Because upstream has moved to a different internal architecture (a mirror-based system), merging its newer work may require porting updaters rather than a clean fast-forward.

## Contribute

If you have suggestions, bug reports, or feature requests, open an issue or submit a pull request.

## License

This project is licensed under the [GPLv3 License](./LICENSE).
