# Home-AssistantConfig
Backup and Step-by-Step configuration of my Home-Assistant.

## Hardware
1x Raspberry Pi 3<br/>
1x Phillips Hue Bridge<br/>
2x Philips Hue White A19 60W Smart Bulb

## Installation
1. Download the [Hassbian Image]<br/>
2. Use [Etcher] to flash the image into the SD card.<br/>
3. Open the `boot` partition and create a new file `wpa_supplicant.conf`. With the following content:<br/>
```
country=DO
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="YOUR_SSID"
    psk="YOUR_PASSWORD"
}
```
4. Insert SD card to Raspberry Pi and turn it on. Initial installation of Home Assistant will take about 10 minutes. After this you should be able to see a new device called `hassbian` in your home network.

[Hassbian Image]: https://github.com/home-assistant/pi-gen/releases/tag/untagged-8e6074a6a6dbf5ed52ca
[Etcher]: https://etcher.io/

### Updating the Host Operating System
SSH to your system as the user `pi` and run:
```
$ sudo hassbian-config upgrade hassbian
```
### Updating Home Assistant
SSH to your system as the user `pi` and run:
```
$ sudo hassbian-config upgrade homeassistant
```
### Expand the Filesystem, change Time Zone and Password
SSH to your system as the user `pi` and run:
```
$ sudo raspi-config
```
### Update and Upgrade Packages
```
sudo apt-get update
sudo apt-get upgrade
```
#### Technical Details
* Home Assistant is installed in a virtual Python environment at `/svr/homeassistant/`
* Home Assistant will be started as a service run by the user `homeassistant`
* The configuration is located at `/home/homeassistant/.homeassistant`

## Installing Samba
`$ sudo hassbian-config install samba`

## Configuration Backup to GitHub
#### Step 1: Installing and Initializing Git
Install the Git package on your Home Assistant server:
```
$ sudo apt-get update
$ sudo apt-get install git
```
#### Step 2: Creating `.GITIGNORE`
Create a `.gitignore` file using `Notepad++` editor and place it in the root of your Home Assistant configuration directory: `<config dir>/.gitignore`.
```
# Example .gitignore file for your config dir
*
!*.yaml
!.gitignore
secrets.yaml
known_devices.yaml
```
#### Step 3: Preparing your Home Assistant Directory for GitHub
```
$ sudo -u homeassistant -H -s
$ cd /home/homeassistant/.homeassistant/
$ git init
$ git config user.email "you@example.com"
$ git config user.name "Your Name"
$ git add .
$ git commit
```