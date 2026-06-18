## Disclaimer:

This is a **modified** boot image and is not a stock file. Flash this at your own risk. I won't take any responsibility for any damage, boot loops, or bricked hardware that may result from flashing this image to your device.

Ensure to flash only on XE-C (2gb) models, I've not tested on XE-A or XE-B

With that said... it works on my end.





If you've rooted your Google Glass XE-C you've probably lost a full gigabyte of ram.

This happens because the standard rooted kernels in circulation (even those provided by Google itself) were compiled with static 1GB memory maps meant for older XE-A/B models.

One solution could be to restore the original firmware, with it's full 2gb (about 1,5 available to the OS) memory mappings, but then you'll lose root.

This custom boot.img solves both issues. It keeps the stock 2GB-capable kernel (preserving OMAP4430 DMM dual-channel mappings) but grafts a root-enabled adbd binary and debug flags onto the ramdisk. You get the full 2GB RAM back, and still keep ADB running as root by default.

### Before rushing to download and flash, check how much ram your pair of glass sees:

Run this command in your terminal:

```
adb shell cat /proc/meminfo | grep MemTotal
```


- If it returns **`MemTotal: ~600MB`**, you are running a 1GB memory limit.

- If it returns **`MemTotal: ~1.5GB`**, you are already using your full RAM.


### Then verify if you actually have root:

Run this command:

```
adb shell
```

- If you see a `#` prompt (or `root@glass_1`) you already have default ADB root.

- If you see a `$` prompt, type `su` and press `Enter`.
    
    *   If it switches to `#`, you have system-level root.
    
    *   If it returns `su: not found` or `permission denied`, you do **not** have root.

<br>

If either check failed (you have less than 1.5GB of RAM OR you don't have root), then I guess you'll need my kernel image.

<br>

## After downloading check the hash of the file before flashing it:

| **SHA-256** |
|---|
| `25cce8ca2920969bddf1e41be9e60c54334b9cba74cd1048f214a4253009a2cd` |

<br>

You can do it with the following command:

### macOS / Linux:
```
shasum -a 256 boot-patched.img
```

### Windows (cmd)

```
certutil -hashfile boot-patched.img SHA256
```
---

## Flashing
If you own a pair of Glass there's an high chance you already know how to flash... but I'll leave this here just in case you need it.
1.  Put your Glass in fastboot mode (Hold **Camera + Power** for 2s, release Power, hold Camera for 10s, connect USB).
2.  Run:
    ```bash
    fastboot flash boot boot-patched.img
    fastboot reboot
    ```
---
## Verification

1.  Verify memory mapping (should show **~1.4GB - 1.5GB**):

    ```
    adb shell cat /proc/meminfo | grep MemTotal
    ```
    
2.  Verify default root shell (should drop straight to **#**):
    
    ```
    adb shell
    ```
