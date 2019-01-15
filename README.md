# Archmat
## Arch Linux automated installation script

- License: MIT License
- Author: Matteljay
- Language: Bash Unix Shell (>= 4.4)
- Homepage: https://github.com/Matteljay


## Introduction

Archmat is a Linux shell script for the automated installation of Arch Linux. Goals:
- Automate the Arch Linux official [installation guide](https://wiki.archlinux.org/index.php/Installation_guide)

- Reduce the chance of typing errors
Repeatedly providing device node references can be both tedious and dangerous.

- Create a functioning operating system
Having a functioning Cinnamon desktop or server environment provides a solid starting point for Arch Linux


## Disclaimer

This script is designed to directly overwrite a system disk. A small typo on either the user's or developer's part
can lead to irrevocable data loss. It is experimental! Disclaimer:

THIS SOFTWARE IS PROVIDED "AS IS" AND ANY EXPRESSED OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE REGENTS OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.


## Operation

- A minimum number of user input will be gathered by the script:
1. Disk to install to
2. Login password, will also be used for disk encryption (optional)
3. Machine name
4. Whether to use LUKS disk encryption
5. Whether to install a graphical desktop environment

- A default user named `user` will be created with the password you provide.

- Language and country settings can be conveniently edited inside the script.

- Installation log will be written to `archmat.log` inside the temporary installation environment.

- The GRUB boot loader will be installed.


## How to launch

1. Download the latest Arch Linux iso file: [link](https://www.archlinux.org/download/)

2. Write it to an USB drive: [guide](https://wiki.archlinux.org/index.php/USB_flash_installation_media)

3. Copy the `archmat` script to the USB key, you could create a free partition for this.

Alternatively, after running `wifi-menu` on the live media:

    wget https://github.com/Matteljay/archmat/blob/master/archmat

Using `wget` is also quite convenient for testing in a virtual machine.

4. Boot the iso and run the script `sh /path/to/archmat`


## Contact info & donations

See the [contact](https://github.com/Matteljay/archmat/blob/master/CONTACT.md) file.


