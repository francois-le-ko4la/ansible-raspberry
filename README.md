# ansible-raspberry

Setup Ubuntu/Raspberry with Ansible

## Why ?
This script makes it possible to automate the tasks to be performed after a fresh ubuntu installation on Raspberry. You can homogenize the configuration of several environment.

This script allows you to:
- [x] Update/upgrade (disable by default)
- [x] Disable the swap (SD card/SSD ^^)
- [x] Install Pi4 packages (not installed by default/ubuntu)
  - rpi-eeprom
  - raspi-config
  - linux-firmware-raspi2
- [x] Setup postfix with a GMail account
- [x] Setup unattended upgrades
- [x] Setup config.txt (server Vs Desktop)
- [x] Install Docker (server)
- [x] Install Portenaire (server)
- [x] Install Adguard-home (server)
- [x] Install Wireguard (server)
- [x] Install multimedia packages (Desktop)
  - ubuntu-restricted-extras
  - libavcodec-extra
  - vlc
  - libgles2-mesa
  - libgles2-mesa-dev
  - xorg-dev
  - caffeine
  - gnome-shell-extension-caffeine
- [x] Install Caffein / Tor Browser (Desktop)
  - Caffein will be automatically started
  - Tor is added in the favorites

You can use, modify, change, do your coffee,... ON YOUR OWN RISKS !!!
This script is provided in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
Lesser General Public License for more details.

## How ?
### Target env

- Enable `ssh`
```console
sudo apt install openssh-server
```
- Create a `Sudo User`
```console
sudo adduser <username>
sudo usermod -aG sudo <username>
sudo su - <username>
exit
```
In my environment, I use `svc_ansible`.

### Install ansible
Install ansible and requirements:
```console
sudo apt install ansible sshpass
sudo ansible-galaxy collection install community.general
```
### Clone git repository

```console
cd /tmp
git clone https://github.com/francois-le-ko4la/ansible-raspberry.git
```
### Prepare playbooks

- change directory
```console
cd ansible-raspberry
```
- Modify the `hosts` file according to your needs.
```dosini
[server]
192.168.1.250

[adguard]
192.168.1.250

[desktop]
192.168.1.145
```
- Edit group_vars/all.yml
```yaml
### Ansible user to manage target
ansible_user: svc_ansible

### Desktop user
std_user: "internet"
#password is generated and get from /etc/shadow
std_password: '$6$zez9DIWqyC6OYNu2$.6s8qTJPqgIvZnaFDlPlmYZkKb.zCnu3pyHDhShIxR.w3xQbXTTVTAyZ3ysDtag9ahMX8NZG7y.JU4wkqyxPx0'

### Postfix
# You should use an app password : https://support.google.com/mail/answer/185833?hl=en-GB
postfix_smtp_user: "XXX@gmail.com"
postfix_smtp_password: "lmrzrmovuymtrebr"

### Unattended - Email
unattended_mail: "XXX"
```
> In order to setup postfix, you should use an app password : https://support.google.com/mail/answer/185833?hl=en-GB

### Use playbook

Run : `./run.sh` or `ansible-playbook -i hosts playbook.yml -kK -vvv`

I used also 3 tags : packages, configuration, update.
- `ansible-playbook -i hosts playbook.yml -kK` : do everything except the update
- `ansible-playbook -i hosts playbook.yml -kK --tags packages` : install/check the packages
- `ansible-playbook -i hosts playbook.yml -kK --tags configuration` : set/check the configuration
- `ansible-playbook -i hosts playbook.yml -kK --tags update` : update the packages and pi firmware (never by default)

## Ubuntu release
21.04 tested/validated
