## Arch Linux automated installation script

- License: MIT License
- Author: Matteljay
- Language: Bash Unix Shell (>= 4.4)
- Homepage: https://github.com/Matteljay


## Introduction

The purpose of these scripts is to facilitate the automated installation of Arch Linux on an encrypted disk.

- The required user input is minimal. However, due to the modular design of these scripts, one can easily customize the installation to taste.
- For reference, see the Arch Linux official [installation guide](https://wiki.archlinux.org/index.php/Installation_guide)
- Reduce the chance of typing errors. Repeatedly providing device node references can be both tedious and dangerous.
- Create a functioning operating system quickly. Booting into a Cinnamon desktop provides a solid starting point for Arch Linux


## Disclaimer

These scripts are designed to directly overwrite a system disk. A small typo on either the user's or developer's part
can lead to irrevocable data loss. Or destruction of your current system. Disclaimer:

THIS SOFTWARE IS PROVIDED "AS IS" AND ANY EXPRESSED OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE REGENTS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


## Operation

**0pkgs_offline** *(optional)*
- Cleans, rebuilds and checks you local pacman package repository so that it can function as a stand-alone package repository for a functioning Arch Linux system.
Special care is taken to ensure inter-dependency checking among the currently installed packages.
- Copies the whole cache and the databases to the *packages/* folder.

**1builddisk**
- Scans the target block device of your choice for existing LVM2 partitions, un-mounts and thoroughly wipes each segment so that no traces remain for pvcreate, vgcreate and lvcreate
- Partitions and formats the disk to have one 2GB boot partition and the remainder for one luks-crypto container. Also asks for a new luks disk password.
- Partitions the luks-crypto container with LVM2 to accommodate a 60GB root filesystem, 4GB swap and the remainder for the user's home.

**2mountdisk**
- Completely mount your freshly created disk based on nothing but the block device.
- Includes creating root and home folders and mounting those partitions from luks-crypto container.

**3installpackages_archive** *(optional)*
- First installs the Arch base-packages from your offline *packages/* folder.
- Then installs the entire local *packages/* folder.

**3installpackages_net**
- First installs the Arch base-packages from the internet.
- Then installs all the package names from the *packagelists/* folder. Will first check if those packages still exist on the official repositories.

**4setup_cinnamon**
- Will completely configure your new installation for automatic GUI login into a Cinnamon desktop environment.
Finds out based on the luks container what block device and which partition uuids to use for *fstab* table.
Also copies the useful custom config files from *setup_res/* folder.
- Requires you to provide a new machine host name, the wheel username *user* will be created. Also asks for a new user password.

**5umountdisk**
- Neatly un-mounts */mnt* and it's recursive mount points.
- Makes sure the lvm nodes are properly removed from */dev/mapper/*

**wipefast_mnt** *(optional)*
- Mostly for debugging, quickly clears the files from an existing disk, leaving only *lost+found/* and emptied *boot/* and *home/* mount points.


## How to launch

You may skip step 1 and 2 if you already have Arch Linux installed manually. The Archmat scripts can easily operate from an existing installation.

1. Download the latest Arch Linux ISO file: [link](https://www.archlinux.org/download/)

2. Write it to an USB drive, then launch it: [guide](https://wiki.archlinux.org/index.php/USB_flash_installation_media)

3. To make sure you get access to the files from this project and open a root shell:

    sudo pacman -S git
    git clone https://github.com/matteljay/archmat
    cd archmat/
    sudo -s

4. Start running the scripts in order, you may skip number '0' if you plan to install via an online mirror


## Additional post-installation tips and tricks

### Automatic luks disk login without password at boot

    dd if=/dev/urandom of=/crypto_keyfile.bin bs=512 count=8
    chmod 000 /crypto_keyfile.bin
    cryptsetup luksAddKey /dev/sdX2 /crypto_keyfile.bin

    nano /etc/mkinitcpio.conf
    FILES=(/crypto_keyfile.bin)

    arch-chroot /mnt mkinitcpio -p linux

### Add extra magnetic disk mount point

    mkdir -m700 /etc/luks-keys/
    dd if=/dev/urandom of=/etc/luks-keys/magnetic.bin bs=512 count=8
    cryptsetup -v luksAddKey /dev/sdX2 /etc/luks-keys/magnetic.bin

    blkid -o value -s UUID /dev/sdX2
    nano /etc/crypttab
    magnetic_lvm UUID=a249aefb-c4b9-4ef2-849a-8b092be5ba6c /etc/luks-keys/magnetic.bin luks

    blkid -o value -s UUID /dev/mapper/magnetic_lvm
    nano /etc/fstab
    UUID=32c1e6ea-cd35-421c-9ae4-28694f136d84 /media/magnetic_home ext4 rw,noatime 0 2


## Contact info & donations

See the [contact](https://github.com/Matteljay/archmat/blob/master/CONTACT.md) file.


