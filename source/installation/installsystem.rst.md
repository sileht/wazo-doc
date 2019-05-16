# Installing the System

Please refer to the section <span data-role="ref">Troubleshooting
\<troubleshooting\></span> if ever you have errors during the
installation.

There are two official ways to install Wazo:

  - using the official ISO image
  - using a minimal Debian installation and the Wazo installation script

Wazo can be installed on both virtual (QEMU/KVM, VirtualBox, ...) and
physical machines. That said, since Asterisk is sensitive to timing
issues, you might get better results by installing Wazo on real
hardware.

## Requirements

For a small install of about 20 users, less than 20 calls per day:

  - RAM: 2 GiB with 1 GiB swap
  - Storage: 2.5 GiB of storage is a very tight minimum, 8 GiB is
    comfortable

## Installing from the ISO image

  - Download the ISO image. ([latest
    version](http://mirror.wazo.community/iso/wazo-current)) ([all
    versions](http://mirror.wazo.community/iso/archives))
  - Boot from the ISO image, select `Install` and follow the
    instructions. You must select a locale with charset UTF-8.
  - At the end of the installation, if you are installing version 16.13
    or before, you need to configure your system to
    <span data-role="ref">use the Wazo infrastructure
    \<using-wazo-infrastructure\></span>, otherwise some errors might
    occur.
  - Continue by running the configuration wizard.

## Installing from a minimal Debian installation

Wazo can be installed directly over a **32-bit** or a **64-bit** Debian
stretch. When doing so, you are strongly advised to start with a clean
and minimal installation of Debian stretch.

The latest installation image for Debian stretch can be found at
<https://www.debian.org/releases/stretch/debian-installer>.

### Requirements

The installed Debian must:

  - use the architecture `i386` or `amd64`
  - have a default locale with charset UTF-8

In case you want to migrate a Wazo from `i386` to `amd64`, see
<span data-role="ref">migrate\_i386\_to\_amd64</span>.

### Installation

Once you have your Debian stretch properly installed, download the Wazo
installation script and make it executable:

    wget http://mirror.wazo.community/fai/xivo-migration/wazo_install.sh
    chmod +x wazo_install.sh

And run it:

    ./wazo_install.sh

At the end of the installation, you can continue by running the
configuration wizard.

### Alternatives versions

The installation script can also be used to install a specific version
of Wazo. For example, if you want to install Wazo 16.16:

    ./wazo_install.sh -a 16.16

You may also install development versions of Wazo with this script.
These versions may be unstable and should not be used on a production
server. Please refer to the usage of the script:

    ./wazo_install.sh -h

## Other installation methods

It's also possible to install Wazo by PXE. More details available on our
blog:

  - <http://blog.wazo.community/around-xivo-describe-industrial-installation-process.html>
  - <http://blog.wazo.community/around-xivo-pxe-server-setup.html>
  - <http://blog.wazo.community/around-xivo-pxe-and-preseeding.html>
