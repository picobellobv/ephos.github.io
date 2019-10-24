---
title: PowerShell Previews on Arch Linux
teaser: When in doubt, makepkg it out!
category: powershell
tags: [powershell, linux]
published : true
reddit_post: https://ephos.github.io/posts/2019-10-13-PowerShell-Previews-on-Arch
featured_comments: 
  - url: 'https://www.reddit.com/r/PowerShell/comments/dm0vkj/powershell_previews_on_arch_linux/f4vollv/'
---

## Installing the Latest PowerShell Previews on Arch Linux

### Table of Contents

- [Installing the Latest PowerShell Previews on Arch Linux](#installing-the-latest-powershell-previews-on-arch-linux)
  - [Table of Contents](#table-of-contents)
  - [Intro](#intro)
  - [Tweaking and Manually Installing an AUR Package](#tweaking-and-manually-installing-an-aur-package)
    - [Step 1 - Clone Down the AUR Package](#step-1---clone-down-the-aur-package)
    - [Step 2 - Updating the PKGBUILD File](#step-2---updating-the-pkgbuild-file)
    - [Step 3 - makepkg and Install it!](#step-3---makepkg-and-install-it)

### Intro

I like to always try to use the preview version of PowerShell as I enjoy getting to use the new features early.  One occasional challenge is that running Arch Linux I need to use the [AUR][AUR] (Arch User Repository) to install the preview versions.  These packages are maintained by a community user and sometimes they can fall behind.  Any consumer of an AUR package should let the maintainer know when their package is out of date, but getting the latest doesn't require waiting for them to update!

### Tweaking and Manually Installing an AUR Package

There is great documentation in the [Arch Wiki][Arch Wiki AUR] on how Arch Linux packages are constructed.  A package contains a _PKGBUILD_ file, which is a shell script that contains the build information needed by Arch Linux to install the package.  This file is consumed by the `makepkg` script which is included with `pacman`, so if you're running on Arch, `makepkg` should be there.

The first thing that needs to be done is to get the package from the AUR.  The two most common ways are to download and extract the tarball, or use git to clone it.  I find Git tends to be the better option, we'll clone down [**powershell-preview-bin**][AUR_PowerShellPreview] with that method.

#### Step 1 - Clone Down the AUR Package

Clone down the package from the AUR.

```bash
# Clone down the powershell-preview-bin AUR package
git clone https://aur.archlinux.org/powershell-preview-bin.git
```

Once cloned down we'll need to modify the _PKGBUILD_ file.  This is pretty a pretty standard step and generally it's good to review the _PKGBUILD_ even if you don't intend to modify it to make sure evil doers and bad guys didn't hide anything malicious in them!

```bash
# Navigate into the clone Git repository for the AUR package.
cd powershell-preview-bin/
#  Since this file is relatively small I am just going to cat it.
cat ./PKGBUILD
```

#### Step 2 - Updating the PKGBUILD File

Reviewing the PKGBUILD we can see this one is fairly cut and dry.  We have some metadata about the package at the top, followed by a simple Bash script in the bottom half.

```bash
# Maintainer: Thiago França da Silva <packagemaintainer@email.com>

pkgname=powershell-preview-bin
_pkgver=7.0.0-preview.3
_version=7-preview
pkgver=${_pkgver/-/.}
pkgrel=1
pkgdesc='A cross-platform automation and configuration tool/framework (binary preview package)'
arch=('x86_64')
url='https://github.com/Powershell/Powershell'
depends=('libunwind' 'icu' 'openssl-1.0')
provides=('powershell')
options=(staticlibs !strip)
install=powershell-preview.install
sha256sums=('4BFE8E86BFD652774C2AE8A35D5B3937CB80E35671C215146C5DF7B6B3A24609')
source=("https://github.com/PowerShell/PowerShell/releases/download/v${_pkgver}/powershell-preview_${_pkgver}-1.ubuntu.18.04_amd64.deb")

package() {
  bsdtar xf data.tar.gz

  mv usr "${pkgdir}"
  mv opt "${pkgdir}"

  cd "${pkgdir}"
  cp -r usr/local/share usr
  rm -rf usr/local

  chmod 755 opt/microsoft/powershell/$_version/pwsh
}
```

In looking through this code, I know there are at a minimum 2 things we need to change to get the latest `pwsh-preview` package installed.  Since the Bash script uses variables from the top section the actual script doesn't need any modification, just the top.

1. `_pkgver=7.0.0-preview.3`
   1. Changing this to the latest preview will feed the right version as a variable to the "source" line.
2. `sha256sums=('4BFE8E86BFD652774C2AE8A35D5B3937CB80E35671C215146C5DF7B6B3A24609')`
   1. Since this checksum is for **7.0.0-preview.3** it will need to be updated to checksum for the version we specify.

Knowing this let's change these two lines.  The first part is easy, the following line...

```bash
_pkgver=7.0.0-preview.3
```

Needs to change to (*at this time PowerShell 7.0 Preview 4 is the latest verison*)...

```bash
_pkgver=7.0.0-preview.4
```

In addition we need to update the checksum.  If you look at the source line...

```bash
source=("https://github.com/PowerShell/PowerShell/releases/download/v${_pkgver}/powershell-preview_${_pkgver}-1.ubuntu.18.04_amd64.deb")
```

You can see this AUR package uses the "**ubuntu.18.04_amd64.deb**" deb package.

Checking the [release][Powershell_Release] page for PowerShell 7.0, you'll see the "**ubuntu.18.04_amd64.deb**" package for PowerShell 7.0 Preview 4 has the following checksum...

`FE7D23C4301F8E2FF890ECF1E9B3398F0F2EB063253D35E6C44F5FFC87D98D65`

*Here it what it looks like on Github.*
![PS7P4-ReleaseChecksum]({{ site.baseurl }}/images/2019-10-13-PowerShell-Previews-on-Arch/PS7P4-ReleaseChecksum.png)

This means we need to update the following line for the checksum from...

```bash
sha256sums=('4BFE8E86BFD652774C2AE8A35D5B3937CB80E35671C215146C5DF7B6B3A24609')
```

to...

```bash
sha256sums=('FE7D23C4301F8E2FF890ECF1E9B3398F0F2EB063253D35E6C44F5FFC87D98D65')
```

After changing those two lines all together it should look like the example below.  Edit the *PKGBUILD* file in your editor of choice (*vi, vim, nano, ed, emacs, vscode, you get the point*).

```bash
# Maintainer: Thiago França da Silva <packagemaintainer@email.com>

pkgname=powershell-preview-bin
_pkgver=7.0.0-preview.4
_version=7-preview
pkgver=${_pkgver/-/.}
pkgrel=1
pkgdesc='A cross-platform automation and configuration tool/framework (binary preview package)'
arch=('x86_64')
url='https://github.com/Powershell/Powershell'
depends=('libunwind' 'icu' 'openssl-1.0')
provides=('powershell')
options=(staticlibs !strip)
install=powershell-preview.install
sha256sums=('FE7D23C4301F8E2FF890ECF1E9B3398F0F2EB063253D35E6C44F5FFC87D98D65')
source=("https://github.com/PowerShell/PowerShell/releases/download/v${_pkgver}/powershell-preview_${_pkgver}-1.ubuntu.18.04_amd64.deb")

package() {
  bsdtar xf data.tar.gz

  mv usr "${pkgdir}"
  mv opt "${pkgdir}"

  cd "${pkgdir}"
  cp -r usr/local/share usr
  rm -rf usr/local

  chmod 755 opt/microsoft/powershell/$_version/pwsh
}
```

#### Step 3 - makepkg and Install it!

Now that we've modified the parts needed to grab the latest preview file from Github and adjusted the checksum for it building it is as easy as running `makepkg`.

```bash
# Run a makepgk to build it with -i to install it.
makepkg -i
```

The gif below shows the updated *PKGBUILD* as well as the `makepkg` process.  In this case running `makepkg -i` will also install the package we are building from the *PKGBUILD* once it's built.  Since it has to copy files to parts of the filesystem that require root access it prompts when needed.

![makepkg]({{ site.baseurl }}/images/2019-10-13-PowerShell-Previews-on-Arch/makepkg.gif)
*This gif is a bit over a minute, but shows the edited PKGBUILD and makepkg install.  Also my gif utility AND/OR i3 was causing some flickering, please bear with it!*

Just like that, PowerShell 7.0 Preview 4 is installed. The main take away, it's always a good idea to remind the AUR package maintainer that there is an update available!  

I acknowledge most folks using Arch Linux may already know how to do this, and anyone using a Microsoft supported distribution for PowerShell 6+ can use whatever the built in package manager is.  If you were an Arch Linux user and curious on how to get the latest and greatest PowerShell 7.0 preview, hopefully this helps!

[AUR]:https://aur.archlinux.org/
[Arch Wiki AUR]:https://wiki.archlinux.org/index.php/Arch_User_Repository
[AUR_PowerShellPreview]:https://aur.archlinux.org/packages/powershell-preview-bin/
[Powershell_Release]:https://github.com/PowerShell/PowerShell/releases/tag/v7.0.0-preview.4