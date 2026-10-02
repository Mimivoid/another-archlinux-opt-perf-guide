## Disclaimer
This is a work in progress.
## Where to start?

### Repositories

In archlinux with pacman, they have hierarchy for repos in /etc/pacman.conf meaning the higher up the repo is the more priority it has.

Note after installing the repos, please for security purposes edit these files in your /etc/pacman.d/ directory

**cachyos-yourarch-mirrorlist** set this to an https server in your country

**alhp-mirrorlist** set this to an https server in a global context

**CachyOS**

[
https://wiki.cachyos.org/features/optimized_repos/#adding-our-repositories-to-an-existing-arch-linux-install](url)

**Arch Linux High Performance**

[https://github.com/an0nfunc/ALHP#quick-start](url)

When you're done installing the repositories, configure /etc/pacman.conf to look like this:

```
[cachyos]
Include = /etc/pacman.d/cachyos-v3-mirrorlist

[cachyos-core]
Include = /etc/pacman.d/cachyos-v3-mirrorlist

[cachyos-extra]
Include = /etc/pacman.d/cachyos-v3-mirrorlist

[multilib-x86-64]
Include = /etc/pacman.d/alhp-mirrorlist

[core]
Include = /etc/pacman.d/mirrorlist

[extra]
Include = /etc/pacman.d/mirrorlist

[multilib]
Include = /etc/pacman.d/mirrorlist
```

Note, I've included the original arch repositories because of certain missing packages.

### Kernel

I recommend installing **linux-cachyos** and **linux-cachyos-headers** then running the following command to ensure it's in grub

Note this command varies depending on how you've installed grub

```
sudo grub-mkconfig /boot/grub/grub.cfg
```

### Wayland

#### Enabling Vulkan on a Compositor

Certain Compositors have backends that are not compatible with Vulkan for example Kwin

Wl-roots based Compositors do have compatibility with Vulkan as an example

To enable Vulkan for example on wl-roots based compositors

Edit /etc/environment and append to the file

```
WLR_RENDERER=vulkan
```

### GRUB Configuration

Edit /etc/default/grub and append to GRUB_CMDLINE_LINUX_DEFAULT

```
tsc=reliable clocksource=tsc
```

No kernel logging grub options

```
loglevel=0 quiet audit=0
```

For AMD cpu's (Zen 2 and above)

```
amd_pstate=guided
```
