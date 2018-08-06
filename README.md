## Intro

Let's directly to the points without emphesizing how usually people get frustrated on flashing JetPack into NVIDIA TX2 without access to an actually Ubuntu machine.

Therefore, this is all about how to flash into TX2 with an VM (VirtualBox) on Mac.

## 1. Pre-requisites

- [X] A mac desktop connected with Wi-Fi

- [X] macOS Sierra 10.12.5 + (Not necessary this version, but this is what I tested)

- [X] access to internet and power (You can't do this locally)

- [X] NVIDIA TX2

- [X] A monitor, HDMI Cable, Ethernet Cable, one Keyboard (USB plugged in), one mouse (preferrable), one USB hub (preferrable)

## 2. Install Virtual Box and Extensions

#### 1.1 Install Virtual Box

Download Virtual Box from [here](https://www.virtualbox.org/wiki/Downloads) and install it first; 

Then download Virtual Box extension [here](https://www.virtualbox.org/wiki/Downloads) and install it;

Extension is needed to enable USB-2 connection/communications between any physical USB device and the virtual machine.

#### 1.2 Spin up an Ubuntu VM

Download Ubuntu 16.04 iso image from [here](http://releases.ubuntu.com/xenial/).

Then, create an ubuntu machine with following settings:

- [X] Storage is larger than 50GB

- [X] Go to Settings --> Network --> Adapter 1, change `Attached to` to `Bridged Adapter`, and name to whatever under Wi-Fi

- [X] Go to Settings --> Ports --> USB, ensure `Enable USB Controller` is under `USB 2.0 (EHCI) Controller` (Enabling USB3 seems to often cause problems when flashing from a VM)

Last, load the image that you just downloaded and spin up an VM. Changes to storage in the VM won't affect actual OS storage, so allow the overwriting of data when prompted during install.

#### 1.3 Download NVIDIA JetPack

In the VM, open firefox browser and go to NVIDIA's official [website](https://developer.nvidia.com/embedded/jetpack) and join them as a member so that you can download the JetPack.

#### 1.4 Install JetPack

Open a terminal and navigate to Downloads folder, then change .run file as executable:

```
cd
cd ~/Downloads
chmod +x ./JetPack-L4T-3.1-linux-x64.run
```

Then, run the .run file:

```
./JetPack-L4T-3.1-linux-x64.run

```

Choose appropriate install paths when prompted. (Note: on an initial install, I changed the paths from the ones there by default -- into the downloads folder. This worked fine. Made a mistake when first installing that required me to reinstall, but would throw an error if I specified a path other than the ones there by default, unlike before. May have to install to default paths if installer is throwing errors).
Selected TX2 when presented with multiple boards.
Install everything in the Components Manager when presented. Enter password to allow the installation if prompted. Installation will likely take a long time.
When prompted with a window "Depending on the component selection, please pay attention to the prompt in the embedded terminal," select OK

Then just follow all the steps (basically from 2 to 12 without 9 in [here](http://docs.nvidia.com/jetpack-l4t/2_1/content/developertools/mobile/jetpack/jetpack_l4t/2.0/jetpack_l4t_install.htm)): choose TX2, L4T 3.1, full-install, then it will take a while to downloads all the dependencies and then a window will pop up to ask you to confirm the installation.

And when download/installation on host machine is finished, you just click all the way to pop up a terminal says : 

![Post Installation](http://docs.nvidia.com/jetpack-l4t/2_1/content/developertools/mobile/jetpack/images/jetpack_l4t_force_recovery_mode.001_600x364.png)


And now, here's the tricky part:
- [X] BEFORE following instructions printed on screen,  open settings of your VM, and go to Settings --> Ports --> USB, and click the usb icon that, when hovered over, reads: 'Add new USB filters with all .....', then add 'NVIDIA Corp. APX'

- [X] Plug in the USB with a USB hub, then connect a mouse, keyboard to TX2

- [X] Connect via HDMI (HDMI-HDMI, no VGA adapters ok?) between TX2 and monitor so that you can see what's going on

- [X] Follow the instructions printed to the terminal. Ensure you are using the same micro USB cord that comes with the TX2 (other cords should work fine, but when I ran into trouble, some suggested a similar cable that fits in the micro USB port but doesn't work may the issue (perhaps it only has power and ground lines for charging and no data transfer lines))

- [X] Last, go back to your VM, do `Press Enter` on the terminal

Flashing will take around 30 minutes.

#### 1.5 Validation Flash

Go to TX2, open terminal:

```
# Validate NVCC
nvcc -V

# Validate cuda

ls /usr/local | grep cuda

# Run example

# You can find this easily online, just google it.
```

Ta Da!
