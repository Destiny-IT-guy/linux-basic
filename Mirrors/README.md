# Mirror

Why do mirror exist?


 Before you used to have to install every application manually. That means you had to go to the application official site, download packages, unzip/untar them and download it. You would usually use the following command to accomplish it:

```bash
wget
tar
chmod x
./
```

These commands download the packages from official site, extract the files, give yourself execution permission to the script, and run it.

This is very tedious. Package manager were created to automate this process by handling downloads, installation, updating, removal and resolving dependencies. Package manager retrieve application from trusted repositories and install them with a simple command.<br>

If all package managers downloaded software from a single server, it would quickly become overloaded and slow. To solve this problem, repositories are copied across multiple servers called mirrors, allowing users to download software from nearby locations and ensuring fast and reliable access.

Common linux package manager include **APT** on debian base system, **DNF/YUM** on Fedora and Red Hat-base distributions and Pacman on ARCH linux. These are official distribution package manager. Linux also support universal packaging system like ***Snap**, **Flatpak** and more. These formats are very useful because they work across multiple distributions, but applications installed this way usually take up more disk space since they bundle their dependencies.

All of these package managers download software from trusted repo, which are hosted on servers around the world. To make downloads faster and more reliable, these repo's are copied to mutiple server called mirrors. When you install or update software, you package manager gets it from a nearby mirror instead of getting it from a single centralize server.

## Configuring mirror

Mirrors are usually configured by default on your system but sometimes they arent the best. Do the following to manually change you mirror on debian base distros.

Go to `/etc/apt/sources.list` or `/etc/apt/sources.list.d/ubuntu.sources`

you will probably see something like this:

```bash
Types: deb
URIs: http://ca.archive.ubuntu.com/ubuntu/
Suites: noble noble-updates noble-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: http://security.ubuntu.com/ubuntu/
Suites: noble-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

Modify the URIs that doesnt' contain security word in it.

Finally use the `sudo apt command` to save file and update package list.

### Where to find mirror

The easiest way to find is go on your distribution official site and check for ones that are up to date. You should also choose mirrors maintained by official organization or universities because they are often up to date and synchronized with the main repository.
