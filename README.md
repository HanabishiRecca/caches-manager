## Caches manager

A simple script that helps keep selected directories clean by symlinking or mounting to tmpfs ramdisk.  
Useful for temporary, cache and other junk directories.  

### Usage

1. Enable system and/or user services.

        # systemctl enable caches-manager.service
        $ systemctl --user enable caches-manager.service

2. Make your configs.

The script reads all `.conf` files from respective directories:  
- `/etc/caches-manager/system` - system-wide configs.
- `/etc/caches-manager/user` - configs for all users.
- `$XDG_CONFIG_HOME/caches-manager` (`~/.config/caches-manager`) - per-user configs.

Configs are read as one path per line. Default mode of operation is to symlink the directory.  
If you want to use mount instead, prefix the path with `b:`. Mounts require root privileges but more failsafe and keep actual directories if the service was disabled/uninstalled, so they are preferred for system directories.  
System-wide configs require full absolute paths (e.g. `b:/var/cache`).  
User configs treat paths relative to user home directory (e.g. `.cache` will be resolved to `~/.cache`).  

**WARNING!** Tmpfs keeps data in RAM and drops it on every shutdown or reboot.  
The app will backup existing non-empty directories just in case.  

3. Changes will be applied on the next boot (or user log-in in case of user service). You can force changes by executing the services or running the script manually, but this is not recommended.

### Packages

**Arch Linux**
- [`caches-manager`](https://aur.archlinux.org/packages/caches-manager) <sup>AUR</sup>

### Manual installation

1. Copy `caches-manager` script to some `$PATH` directory, e.g. `/usr/bin`.
2. Copy `systemd` directory to `/usr/lib`.

If you want to use the script without systemd, just make it run early at system startup with `--system` flag and on user log-in with `--user` flag.
