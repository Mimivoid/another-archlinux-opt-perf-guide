## Disclaimer

This is not affiliated with official archlinux things.

This is a work in progress.

Please let me know of any issues you have in the issues section to help me further revise this guide or become apart of building this guide.

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

When you're done installing the repositories, configure /etc/pacman.conf to be a priority like this, although this is an example x86-64 repo layout so ideally set it to your arch.

```
[cachyos]
Include = /etc/pacman.d/cachyos-mirrorlist

[cachyos-core]
Include = /etc/pacman.d/cachyos-mirrorlist

[cachyos-extra]
Include = /etc/pacman.d/cachyos-mirrorlist

[multilib-x86-64]
Include = /etc/pacman.d/alhp-mirrorlist

[core]
Include = /etc/pacman.d/mirrorlist

[extra]
Include = /etc/pacman.d/mirrorlist

[multilib]
Include = /etc/pacman.d/mirrorlist
```

Setting it to your arch for example, if it's x86-64-v3 then

```
[cachyos-v3]
Include = /etc/pacman.d/cachyos-v3-mirrorlist

[multilib-x86-64-v3]
Include = /etc/pacman.d/alhp-mirrorlist
```

After these are installed run this command

```
sudo pacman -Syu
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

### Environment Variables

Edit and append these variables to /etc/environment of your choosing

#### Important variables

Sets the shader cache size in gigabytes
```
MESA_SHADER_CACHE_MAX_SIZE=50G
```

Sets the SDL video driver to wayland
```
SDL_VIDEODRIVER=wayland
```

Set this to a previous, current or later year of when your GPU was made
```
MESA_EXTENSION_MAX_YEAR=2020
```

Sets LIBGL to direct because indirect has performance reductions
```
LIBGL_ALWAYS_INDIRECT=0
```

Sets the mesa vsync layer to off, vsync still works for games
```
vblank_mode=0
```

#### Vulkan Mesa related variables

Generally good to enable for multi-core processors
```
MESA_VK_ENABLE_SUBMIT_THREAD=1
```

Sets the vsync layer off for gaming, vsync still works for games
```
MESA_VK_WSI_PRESENT_MODE=immediate
```

#### RADV related variables

This is for AMD GPU's only

These variables may work, could vary from system to system

```
AMD_VULKAN_ICD=RADV
ACO_DEBUG='novalidate'
RADV_PERFTEST='lowlatencydec,localbos,cswave32,gewave32'
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

Then run

```
sudo grub-mkconfig /boot/grub/grub.cfg
```
