# rpi4tonas-guide

Turn a Raspberry Pi 4 and external disks into a low-cost NAS for scheduled Windows PC backups and remote-server archives. This Raspberry Pi NAS guide covers OpenMediaVault setup, Windows batch backup automation, and an optional browser-accessible Linux desktop.

## Features

- Configure attached storage with OpenMediaVault and make it available on your network.
- Schedule Windows PC backups with `rsync`, including waking the PC before a backup and shutting it down afterward.
- Create compressed archives on a remote server, download them to the NAS, and rotate older backups.
- Adjust backup settings through `configure_all.bat` and use rsync exclude files to omit selected paths.
- Host a persistent Linux desktop through Docker and Webtop for access from a browser.

## Installation

You need a Raspberry Pi 4 with at least 4 GB of RAM, a 16 GB or larger microSD card, a suitable Pi power supply, and external storage connected through USB-to-SATA adapters. Use Ethernet for the initial setup. A powered USB hub may be needed for multiple drives.

**Power and data safety:** A 3.5-inch SATA disk needs an external supply with both 5 V and 12 V; a powered USB hub alone is insufficient. Back up existing disk contents before any formatting step.

1. Use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to install Raspberry Pi OS Lite (64-bit). Check the current [OpenMediaVault](https://www.openmediavault.org/) installation guidance before choosing the OS release.
2. Set a hostname and user account in Imager, enable SSH, and boot the Pi on your wired network.
3. Connect over SSH and install OpenMediaVault according to its instructions for the supported OS release.
4. Configure the attached disks, filesystems, shared folders, and access permissions in OpenMediaVault. Verify that your Windows PC can access the intended backup location.
5. Place the project files in a local Windows directory. In Command Prompt, inspect the available batch files:

```bat
cd /d C:\src\rpi4tonas-guide
dir *.bat
```

The backup workflow also needs `rsync`, network access between the machines, and suitable authentication for remote transfers. Windows does not include `rsync` by default. Wake-on-LAN requires support and configuration on the PC and its network adapter.

## Usage

From the project directory, run the configuration helper:

```bat
cd /d C:\src\rpi4tonas-guide
cmd /c configure_all.bat
```

Here, `/c` is a Windows Command Prompt argument: it runs the batch file and exits. No project-specific command-line flags are documented in the source material.

Review the helper and backup scripts before execution. Set the machine addresses, source paths, destination paths, credentials, and retention settings required by those scripts. Check the rsync exclude files against the data you intend to preserve.

**Test before scheduling:** Use a small source folder first, verify the copied files, and restore a sample file. Review shutdown and rotation behavior before allowing unattended runs; those steps can interrupt work or remove older backups.

For remote-server backups, confirm that the archive contains the expected files before enabling retention cleanup. For the optional desktop, install Docker and configure Webtop separately. Keep OpenMediaVault and Webtop behind your local network or a secured remote-access connection rather than exposing their interfaces directly to the internet.

## License

MIT. This project is based on [Microck/RPi4toNAS-Guide](https://github.com/Microck/RPi4toNAS-Guide). Preserve the original copyright notice and MIT permission notice when redistributing its code or documentation.

## Related projects

The supplied related-project list includes these repositories, but provides no descriptions of their functionality:

- [RedZinXL/warc-gpt](https://github.com/RedZinXL/warc-gpt): Consult its README for scope and usage; its purpose is not established by the supplied material.
- [Akshay-G-Dev/skooner](https://github.com/Akshay-G-Dev/skooner): Consult its README for scope and usage; its purpose is not established by the supplied material.

