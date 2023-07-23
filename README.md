# Fedora
After installing Fedora do these for a more enjoyable experience.

## DNF Configuration
Increase the speed of DNF:
<code>sudo nano /etc/dnf/dnf.conf</code>
<p>Add the following lines to dnf.conf</p>

<ul>
<li>fastestmirror=True</li>
<li>max_parallel_downloads=10</li>
<li>defaultyes=True</li>
<li>keepcache=True</li>
</ul>

### fastestmirror: 

<p>If enabled a metric is used to find the fastest available mirror. </p>

### max_parallel_downloads:

<p>Maximum number of simultaneous package downloads.</p>

### defaultyes:

If enabled the default answer to user confirmation prompts will be Yes.

### keepcache:
Keeps downloaded packages in the cache when set to True. 

## Enable RPM Fusion
RPM Fusion provides software that Fedora doesn't want to ship. That software is provided as precompiled RPMs for all current Fedora versions.

### Command Line Setup using rpm
To enable access to both the free and the nonfree repository use the following command: 
<code>sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm</code>

### AppStream metadata
RPM Fusion repositories also provide Appstream metadata to enable users to install packages using Gnome Software/KDE Discover. Please note that these are a subset of all packages since the metadata are only generated for GUI packages. 
<code>sudo dnf groupupdate core</code>

## Adding Flatpaks
Flatpak is installed by default on Fedora Workstation.
<code>flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo</code>

## Install Media Codecs
### Switch to full ffmpeg
Fedora ffmpeg-free works most of the time, but one will experience version missmatch from time to time. Switch to the rpmfusion provided ffmpeg build that is better supported. You will still need to follow the next section for additional codecs or plugins related to packages you might have installed. 
<code>sudo dnf swap ffmpeg-free ffmpeg --allowerasing</code>
### Install additional codec
This will allows the application using the gstreamer framework and other multimedia software, to play others restricted codecs:
<code>sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin</code>
The following command will install the sound-and-video complement packages needed by some applications:
<code>sudo dnf groupupdate sound-and-video</code>
## Install Nvidia Drivers
### Determining Card Model
NVIDIA has several driver series, each of which has different hardware support. To determine which driver you need to install, you'll first need to find your graphics card model. 
<code>/sbin/lspci | grep -e VGA</code>
### Installing Drivers
<code>sudo dnf install xorg-x11-drv-nvidia-390xx akmod-nvidia-390xx</code>
