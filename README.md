## Caches manager

A simple script that helps keep selected directories clean by symlinking to tmpfs ramdisk. Useful for temporary, cache and other junk directories.  
Systemd services are included to automate this process.  

### Usage

1. Enable both system and user services.

```sh
systemctl enable caches-manager.service
systemctl --global enable caches-manager.service
```

2. Make your configs.

The script reads all `.conf` files from respective directories:  
- `/etc/caches-manager/system` - system-wide configs.
- `/etc/caches-manager/user` - configs for all users.
- `$XDG_CONFIG_HOME/caches-manager` (`~/.config/caches-manager`) - per-user configs.

Configs are read simply as one path per line.  
System-wide configs require full absolute paths (e.g. `/var/cache`).  
User configs treat paths relative to user home directory (e.g. `.cache` will be resolved to `~/.cache`).  

**WARNING!** All content in the target directories will be purged!  
Ensure that the target directories do not contain any valuable data. Tmpfs keeps data in RAM and drops it on every shutdown or reboot.  

3. Changes will be applied on the next boot (or user log-in in case of user service). You can force changes by executing the services manually, but this is not recommended.

### Manual installation

1. Copy `caches-manager` script to some `$PATH` directory, e.g. `/usr/bin`.
2. Copy `systemd` directory to `/usr/lib`.
