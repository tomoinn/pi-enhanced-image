# Pi Base Image - Setup

Get it here: 

This is my enhanced version of Raspberry Pi OS (https://www.raspberrypi.org/software/). I've started with the minimal version from that page and added:

1. Updates applied from ``apt``
2. Several newer version of python, specifically `python3.9.6` and `python3.10.0b4`, compiled from source and installed with `altinstall`, this leaves the existing package managed versions intact.
3. All the necessary libraries to compile Python and OpenCV (including all optional extensions)
4. A build of OpenCV `4.5.3-dev` with optimisations enabled
5. Docker `20.10.8`, with the deamon configured to run as a system service
6. Node.js `v16.6.1`
7. Go `v1.11.6`
8. SSH enabled
9. Default password for `pi` changed to `changeme`
10. Compressed back to minimal size, use `raspi-config` to expand the file system on first use

## Creating Python virtual environments

Use virtual environments to take advantage of the more modern Python versions:

```
python3.9 -m venv my_new_env
source my_new_env/bin/activate
```

## Installing OpenCV into a new virtual environment

OpenCV is installed to `/usr/local/lib/python3.9/site-packages/cv2/python3.9/cv2.so` but should work with the other versions. To install it into a new virtual environment you should symlink e.g. `my_new_env/lib/python3.9/site-packages/cv2.so` to this location, for example, for the `my_new_env` created above you'd do the following, but adjust paths to reflect the actual location and version of your virtual environment's site-packages directory:

```
cd my_new_env/lib/python3.9/site-packages
ln -s /usr/local/lib/python3.9/site-packages/cv2/python-3.9/cv2.so cv2.so
```

## Configuring the hostname

The hostname for this image is `green` by default, because the Pi I'm testing it on is in a green case. You probabably want to change it. By default this makes the Pi visible at `green.local` if your network and client support MDNS (this means I can do e.g. `ssh pi@green.local` and it works without having to mess around finding IP addresses etc. 

### Edit hosts file

```
sudo nano /etc/hosts
```

Change the entry for `127.0.1.1` from `green` to whatever name you'd like

### Edit hostname file

```
sudo nano /etc/hostname
```

Change `green` to whatever name you'd like
