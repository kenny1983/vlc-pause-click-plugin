# Pause Click plugin for VLC
VLC plugin that allows you to pause/play a video by clicking on the video image.

By default it pauses on every click, but it can be configured to work nicely with double-click-to-fullscreen by enabling ["Prevent pause/play from triggering on double click" option in the settings](http://i.imgur.com/Kdrekks.png).

(As an extra functionality, it also allows mouse key re-assignment.
Specifically, it allows disabling fullscreen toggle on double click and context menu toggle on right click, as well as re-assigning them to different mouse buttons, e.g. you could make VLC fullscreen on right or middle mouse buttons.)

## Table of contents

- [Supported versions of VLC](#supported-versions-of-vlc)
- [Install](#install)
  - [Windows](#windows)
  - [macOS](#macos)
  - [Linux](#linux)
    - [Flatpak](#flatpak)
    - [Debian](#debian)
    - [Arch Linux](#arch-linux)
    - [Snap](#snap)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Supported versions of VLC
VLC 2.1, 2.2 and 3.0 are supported.

VLC versions below 2.1 are not supported.
However, there is [a lua extension](https://github.com/3demax/vlc-clickpause) that should work with VLC 2.0 and possibly even earlier versions of VLC.

Support for the newer VLC versions will be available as they get released and as I get time to work on it.

There is an experimental support for the nightly, in-development version of VLC (VLC 4.0).
The development version of VLC often breaks both API and ABI, which can result in the plugin failing to build and the plugin built for nightly VLC of today be non-functional in newer nightly VLC builds.
Due to this, the support for nightly VLC is very limited and low-priority.
The plugin is not guaranteed to work correctly with it, or even work at all.
If the plugin breaks to build against newer nightly VLC versions, it might not get fixed for quite some time.
It's also quite possible that nightly VLC could have its own bugs that prevent the plugin from working that are out of my control to fix.

## Install

### Windows
Download an appropriate archive:

Version/Bitness | VLC 32 bit | VLC 64 bit
----------- | ------ | -------
VLC 2.1 | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-2.1-32bit-win.zip) | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-2.1-64bit-win.zip)
VLC 2.2 | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-2.2-32bit-win.zip) | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-2.2-64bit-win.zip)
VLC 3.0 | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-3.0-32bit-win.zip) | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-3.0-64bit-win.zip)

For the nightly VLC 4.0 [see the nightly builds](https://github.com/nurupo/vlc-pause-click-plugin-nightly-builds).

Extract the archive at `{VLC}\plugins\video_filter\`, where `{VLC}` is the directory the VLC was installed into.
By default, the 32-bit VLC installs into `C:\Program Files (x86)\VideoLAN\VLC\` and the 64-bit one into `C:\Program Files\VideoLAN\VLC\`.

Then follow [the usage instructions](#usage) below on how to enable the plugin.

If you want to build the plugin binary yourself, take a look at the [build instructions](/BUILD.md).

### macOS

Download an appropriate archive:

Version | VLC 64 bit | Universal (works on both x86_64 and arm64)
----- | ----- | -----
VLC 2.1 | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-2.1-macosx.zip) | [Download](https://github.com/nurupo/vlc-pause-click-plugin/files/10227403/vlc-2.1-macosx-intel64.zip)
VLC 2.2 | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-2.2-macosx.zip) | [Download](https://github.com/nurupo/vlc-pause-click-plugin/files/10227402/vlc-2.2-macosx-intel64.zip)
VLC 3.0 | [Download](https://github.com/nurupo/vlc-pause-click-plugin/releases/download/2.2.0/vlc-3.0-macosx.zip) | [Download](https://github.com/nurupo/vlc-pause-click-plugin/files/10227401/vlc-3.0-macosx-universal.zip)

For the nightly VLC 4.0 [see the nightly builds](https://github.com/nurupo/vlc-pause-click-plugin-nightly-builds).

Ensure that VLC opens without any errors or security prompts.

Extract the archive into `/Applications/VLC.app/Contents/MacOS/plugins/`.

Then follow [the usage instructions](#usage) below on how to enable the plugin.

If you want to build the plugin binary yourself, take a look at the [build instructions](/BUILD.md).

### Linux

#### Flatpak
This plugin [is available on Flathub as `org.videolan.VLC.Plugin.pause_click`](https://flathub.org/apps/org.videolan.VLC.Plugin.pause_click).
(The manifest file is available in [flathub/org.videolan.VLC.Plugin.pause_click](https://github.com/flathub/org.videolan.VLC.Plugin.pause_click) repository -- just note that it defaults to an outdated branch.)

If you use Flathub VLC, you can add the plugin to your VLC by installing `org.videolan.VLC.Plugin.pause_click`.
Assuming you have Flathub repo added, you can install the plugin using:
```bash
flatpak install flathub org.videolan.VLC.Plugin.pause_click
```

The above command will ask you which branch of the plugin extension you wish to install.
**You must select the correct branch for your version of Flathub VLC or the plugin will not be available in the VLC.**
Unfortunately, flatpak doesn't indicate in the `flatpak install` output which extension branches work with the already installed apps.
To find the correct plugin extension branch for your version of VLC, you can check the `versions=` field under `[Extension org.videolan.VLC.Plugin]` in the output of `flatpak info --show-metadata org.videolan.VLC`.
The following automates this process for you, installing the correct plugin extension branch for your version of VLC:
```bash
ARCH="$(flatpak info --show-metadata org.videolan.VLC \
      | sed -En '
          /^\[Application\]/ {
              :label
              /^runtime=/ {
                  # Leave just the arch
                  s|[^/]*/||
                  s|/[^/]*||
                  p
                  q
              }
              n
              /^[^\[]/b label
              q
          }'
)"
BRANCH="$(flatpak info --show-metadata org.videolan.VLC \
      | sed -En '
          /^\[Extension org.videolan.VLC.Plugin\]/ {
              :label
              /^versions=/ {
                  # Use the second version in the list. This assumes a specific
                  # "versions=" format and might break if it changes.
                  s/[^;]*;//
                  p
                  q
              }
              n
              /^[^\[]/b label
              q
          }'
)"
flatpak install "runtime/org.videolan.VLC.Plugin.pause_click/$ARCH/$BRANCH"
```

If the correct plugin extension branch is installed, you should see `Extension: runtime/org.videolan.VLC.Plugin.pause_click/$ARCH/$BRANCH` listed in the output of:

```bash
flatpak info flatpak info --show-extensions org.videolan.VLC
```

If you do not have the correct plugin extension branch installed, there will be no `Extension: runtime/org.videolan.VLC.Plugin.pause_click` listed in there.

Once installed, follow [the usage instructions](#usage) below on how to enable the plugin.

Also, note that updating Flathub VLC might remove the plugin and you will need to re-install the plugin again.
Specifically, due to how VLC plugins are published (by using run-time specific branches https://github.com/flathub/flathub/pull/3843#issuecomment-1401719795) and due to Flatpak being unable to automatically update plugins that are published in such a way (https://github.com/flatpak/flatpak/issues/4208), every time VLC updates its runtime, thus changing the required runtime version its plugins must use, the already installed plugins will stop working as they use an older runtime and don't satisfy that requirement, and the versions of plugins that use the new runtime that VLC now requires must be manually installed instead.
Not every VLC update will cause this, only the ones that change the required plugin runtime, which typically happens only once a year or so.

#### Debian
Get required libraries and tools:
```bash
sudo apt-get install build-essential pkg-config libvlccore-dev libvlc-dev
```

Get the latest release of the plugin:
```bash
sudo apt-get install git
git clone https://github.com/nurupo/vlc-pause-click-plugin
cd vlc-pause-click-plugin
git checkout \
  "$(git tag --list | grep -P '^(\d+).(\d+).(\d+)$' | \
    sed "s/\./ /g" | \
    sort -snk3,3 | sort -snk2,2 | sort -snk1,1 | \
    tail -n 1 | \
    sed 's/ /\./g')"
```

(Or alternatively download [the latest release's tarball](https://github.com/nurupo/vlc-pause-click-plugin/releases/latest), extract it and cd into it)

Build and install:
```bash
make
sudo make install
```

Then follow [the usage instructions](#usage) below on how to enable the plugin.

If these build instructions don't work for you (perhaps you are using a non-Debian-derived Linux distribution), [there are more generic build instructions available](/BUILD.md).

#### Arch Linux
There is [`vlc-pause-click-plugin` package](https://aur.archlinux.org/packages/vlc-pause-click-plugin/) available in the AUR repository.

Once installed, follow [the usage instructions](#usage) below on how to enable the plugin.

#### Snap
Snap VLC is not supported.
Not supported in the sense that I haven't figured out how to build the plugin such that it would be ABI compatible with the Snap VLC and how to make the Snap VLC load the plugin.
If someone could figure that out and contribute their findings - that would be very helpful.
[This Snap VLC discussion](https://github.com/nurupo/vlc-pause-click-plugin/issues/33) might be useful.

## Usage
1. Restart VLC to load the newly added plugin [[screenshot]](http://i.imgur.com/G2QAK17.png)
2. Go into advanced preferences: Tools -> Preferences -> Show settings -> All [[screenshot]](http://i.imgur.com/QqORpID.png) [[screenshot]](http://i.imgur.com/RWLYX5g.png)
3. Enable the plugin with a checkbox: (in advanced preferences) Interface -> Control Interfaces -> Pause/Play video on mouse click [[screenshot]](http://i.imgur.com/m9yF5Px.png)
4. Enable the plugin with a checkbox: (in advanced preferences) Video -> Filters -> Pause/Play video on mouse click [[screenshot]](http://i.imgur.com/OZLqmI6.png)
5. Change the plugin settings however you like: (in advanced preferences) Video -> Filters -> Pause click [[screenshot]](http://i.imgur.com/Kdrekks.png)
6. Restart VLC for settings to take place [[screenshot]](http://i.imgur.com/G2QAK17.png)
7. Play a video
8. Click on the video picture to pause/play the video

Make sure you have checked both of "Pause/Play video on mouse click" checkboxes per \#3 and \#4!
It's a very common mistake to skip one of those.

## Troubleshooting

### I don't see any plugin-related options described in [Usage](#usage) instructions in my VLC preferences

- Make sure to restart VLC after you have put the plugin `.dll` (Windows) or `.so` (Linux) or `.dylib` (macOS) in the appropriate directory.
- Make sure you have downloaded the appropriate version of the plugin for the VLC you are trying to use it with.
Plugin's version and bitness should match the version and bitness of the VLC you are trying to use the plugin with.
Plugin's bitness has nothing to do with the operation system bitness.
For example, if you use 32-bit VLC, it doesn't matter whether your Windows is 32-bit or 64-bit, you should use 32-bit version of the plugin, because it matches the bitness of the VLC you use.
- Some users [have reported](https://github.com/nurupo/vlc-pause-click-plugin/issues/32#issue-296248669) that they had to run the VLC executable with `--reset-plugins-cache` flag once for the plugin to appear in the GUI.

### I do see the plugin-related options described in [Usage](#usage) instructions in my VLC preferences, but the plugin still doesn't work

- Double-check that you have followed [Usage](#usage) instructions closely, especially 3rd and 4th points.
Look at the screenshots.
- Some users [have reported](https://github.com/nurupo/vlc-pause-click-plugin/issues/45#issuecomment-418964880) that they had to re-install VLC and select "Delete preferences and cache" in the installer for the plugin to work.

If the issue persists, open an Issue in this repository and I will try to help.

### Green video image

When playing interlaced videos in VLC 3.0 on Windows using Direct3D11 hardware-accelerated decoding, the first interlaced video will play fine, but the following ones will play showing a green video image (while still correctly playing the sound).

This is a known bug in VLC 3.0 and it has to be fixed in VLC, it's not something that can be fixed in our plugin. The upcoming VLC 4.0 has this bug fixed, however it's unlikely that the fix will be back-ported to VLC 3.0.

[You can see this issue report for more information.](https://github.com/nurupo/vlc-pause-click-plugin/issues/58)

There are several possible workarounds:

- Enable "Direct3D11 deinterlace filter" in: Tools -> Preferences -> (All) -> Video -> Filters

  Note that this results in x2 GPU usage due to additional video format conversions this option makes.

- Disable hardware-accelerated decoding.

  You can do so in: Tools -> Preferences -> (Simple) -> Input / Codecs -> Hardware-accelerated decoding

  Note that this results in higher CPU usage, since the video is now decoded in software using the CPU.

- Restart VLC every time you want to play a second interlaced video in a VLC session.

- If you have Intel integrated graphics, you could try making VLC use that for its hardware-accelerated decoding.

### It works for videos but not for audio-only files

That's not a bug, that's the expected behavior due to how the plugin and VLC work.

To know when a user clicks their mouse, the plugin implements the video filter interface of VLC, which allows it to react on mouse clicks done on the video surface/image.
When playing audio-only files, VLC doesn't create any video surface and thus doesn't load any of video filter plugins, so the plugin simply doesn't get loaded and can't react to mouse clicks.

There is, however, a creative way to make the plugin work on audio-only files -- enable an audio visualization via Audio -> Visualizations menu.
Enabling an audio visualization will force VLC to create a video surface for the visualization and load the plugin.
Now, if you click on the visualization, the audio should pause.
All visualizations seem to work aside from the 3D spectrum one.
If you are on Linux, depending on how VLC is packaged in your distribution, you might need to install an additional package to enable audio visualizations, despite them already being listed in the GUI (e.g. `vlc-plugin-visualization` package in Debian).

## License
LGPL-2.1-or-later
