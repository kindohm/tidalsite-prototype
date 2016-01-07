# Installation

## Linux installation script

If you use Debian, or a related distribution such as Ubuntu, the
following script should install tidal, dirt and its dependencies for
you. Follow the link, then click 'raw' to download the file:

  <https://github.com/yaxu/Tidal/blob/master/doc/install-linux.sh>

Then open a terminal window, and type the following

~~~~sh
cd ~/Downloads
bash install-linux.sh
~~~~

You'll need to change `~/Downloads` to the location that your web
browser saved the `install-linux.sh` file.

Once you have run the script, log out and then back in again (to
ensure you have the right group settings), and if all is well, you
should be good to skip ahead to "Starting dirt".

## Linux installation by hand

Unless otherwise specified, you will need to run the commands in a
terminal window.

### Installing Dirt by hand

Tidal does not include a synthesiser, but instead communicates with an
external synthesiser using the Open Sound Control protocol. It has
been developed for use with a particular software sampler called
"dirt". You'll need to run it with "jack audio".

Here's how to install dirt under Debian, Ubuntu or a similar distribution:

~~~~sh
sudo apt-get install build-essential libsndfile1-dev libsamplerate0-dev \
                     liblo-dev libjack-jackd2-dev qjackctl jackd git
git clone --recursive https://github.com/tidalcycles/Dirt.git
cd Dirt
make clean; make
~~~~

### Installing Tidal by hand

Tidal is embedded in the Haskell language, so you'll have to install
the haskell interpreter and some libraries, including tidal
itself. Under debian, you'd install haskell like this:

~~~~sh
sudo apt-get install ghc zlib1g-dev cabal-install
~~~~

Or otherwise could grab the [haskell platform](http://www.haskell.org/platform/)

Once Haskell is installed, you can install tidal like this:

~~~~sh
cabal update
cabal install tidal
~~~~

You will have to start dirt every time you want to run Tidal,
otherwise there will be no sound.

Now you need to install and configure an editor, following the
instructions below. For beginners, the Atom editor is recommended.

### Starting Dirt

Before you use Tidal, you'll need to start the Dirt synth.

First of all, start the "jack" audio layer. The easier way to do this
is with the "qjackctl" app, which you should find in your program
menus under "Sound & Video" or similar. If you have trouble with
qjackctl, you can also try starting jack directly from the
commandline:

~~~~sh
jackd -d alsa &
~~~~

If that doesn't work, you might well have something called
"pulseaudio" in control of your sound. In that case, this should work:

~~~~sh
/usr/bin/pasuspender -- jackd -d alsa &
~~~~

And finally you should be able to start dirt with this:

~~~~sh
cd ~/Dirt
./dirt &
~~~~

If you have problems with jack, try enabling realtime audio, and
adjusting the settings by installing and using the "qjackctl"
software. Some more info can be found in the [Ubuntu Community page for JACK configuration](https://help.ubuntu.com/community/HowToJACKConfiguration)

**You will have to start dirt every time you want to run Tidal,
otherwise there will be no sound.**


## OSX

Installing Tidal's dependencies on Mac OS X can be done via homebrew or MacPorts, but choose only one to avoid conflicts with duplicate system libraries.

Unless otherwise specified, the below commands should be typed or pasted into a terminal window.

### Dirt

### Installing dependencies via Homebrew
[Homebrew](http://brew.sh) is a package manager for OS X. It lives side by side with the native libraries and tools that ship with the operating system.

To install homebrew:

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Initialise homebrew:

```bash
brew doctor
```

Install Dirt, a synth (well, more of a sampler) made to work with
Tidal. A homebrew 'recipe' for dirt does exist, but that doesn't come
with any sounds to play with, so for now it's probably easiest just
download it all from github and compile it as follows.

Install some libraries which the Dirt synth needs to compile:

```bash
brew install liblo libsndfile libsamplerate
```

Install the 'jack audio connection kit' which Dirt also needs:

```bash
brew install jack
```

{% capture jackosx %}
__Note:__ If Homebrew's installation of Jack fails with a `make` error, you can use the [JackOSX Installer](http://www.jackosx.com/download.html) instead. This will, however, add an additional step when installing Dirt (see below).
{% endcapture %}
{% include alert.html content=jackosx %}


### Alternative: Installing dependencies via Mac Ports
[MacPorts](https://www.macports.org/) is another package manager for OS X.
If you already installed dependencies via homebrew, skip ahead to build Dirt.
Otherwise if you happen to already use MacPorts, here's a list of steps in order to get all dependencies:

```bash
sudo port install liblo libsndfile libsamplerate
```

Download and install jack2 [Jack Download Page](http://jackaudio.org/downloads/). Jack 2 has better OS X integration [Jack Comparison](https://github.com/jackaudio/jackaudio.github.com/wiki/Q_difference_jack1_jack2).

### Building Dirt from source

Get the source code for the Dirt synth:

```bash
cd ~
git clone --recursive https://github.com/tidalcycles/Dirt.git
```

Compile dirt:

```bash
cd ~/Dirt
make clean; make
```

{% capture dirtcompile %}
If Dirt fails to compile after using the JackOSX installer as above, you may need to add flags to the Makefile to specify the appropriate paths:

```make
CFLAGS += -g -I/usr/local/include -Wall -O3 -std=gnu99 -DCHANNELS=2
LDFLAGS += -lm -L/usr/local/lib -llo -lsndfile -lsamplerate -ljack
```
{% endcapture %}
{% include alert.html content=dirtcompile caption="Homebrew users" %}

{% capture dirtcompileport %}
As MacPorts installs all libs on `/opt/local/`
edit the Makefile to point the right direction of `libsndfile` and `libsamplerate`

```make
CFLAGS += -g -I/opt/local/include -Wall -O3 -std=gnu99
LDFLAGS += -lm -L/opt/local/lib  -llo -lsndfile -lsamplerate
```
{% endcapture %}
{% include alert.html content=dirtcompileport caption="MacPorts users" %}


### Haskell

get the binary installer for [the haskell platform](https://www.haskell.org/platform/mac.html).
Or you might get it from homebrew (this takes a while)

```bash
brew install haskell-platform
```

Or, if using MacPorts:

```bash
sudo port install haskell-platform
```

Install Tidal itself:

```bash
cabal update
cabal install cabal-install
cabal install tidal
```

Now you have to start dirt, the synthesizer/sampler, before getting a
code editor going. So back in a terminal window, start jack:

```bash
jackd -d coreaudio &
```

Or, if you downloaded Jack 2, then start the JackPilot at:
/Applications/Jack/JackPilot.app
Click __start__ button.

Then start dirt:

```bash
cd ~/Dirt
./dirt &
```

You will have to start dirt every time you want to run Tidal,
otherwise there will be no sound.

Now you need to install and configure an editor, following the
instructions below. For beginners, the Atom editor is recommended.



## Windows

This is a rough set of instructions based on information from the
Tidal mailing list. There is a [fine youtube
video](https://www.youtube.com/watch?v=KPCRUuYsb5M) that takes you
through this process step-by-step.


### Cygwin
First, install [Cygwin](https://www.cygwin.com). In Cygwin, make sure the
following packages are installed:

~~~~
git
gcc-core
make
gcc-g++
libsndfile
libsndfile-devel
libsamplerate
libsamplerate-devel
~~~~

### Portaudio

Download Portaudio from http://www.portaudio.com. In Cygwin, Unpack
the download with `tar fxvz`. After unpacking, from Cygwin, go to the directory
where you unpacked Portaudio and then run:

~~~~bash
./configure && make && make install
~~~~

### Liblo

Download [Liblo](http://liblo.sourceforge.net).
In Cygwin, unpack Liblo with `tar fxvz`, then in Cygwin go to the directory where you
unpacked Liblo and then run:

~~~~bash
./configure && make && make install
~~~~

### Dirt

In Cygwin:

~~~~bash
git clone --recursive http://github.com/tidalcycles/Dirt.git
~~~~

Then:

~~~~bash
cd Dirt
make dirt-pa
~~~~

Then you get a `dirt-pa.exe` that works. Maybe this even works on any
windows system without having to compile. You'd need `cygwin1.dll` at
least though.

### Haskell

[Download and install the haskell platform](https://www.haskell.org/platform/).

*You will then either need to restart Cygwin or reload your Cygwin profile to re-load `cabal` on your path.*

Then back in Cygwin:

~~~~bash
cabal update
~~~~

At this point, you may be notified to update `cabal-install`. You can do so by typing:

~~~~bash
cabal install cabal-install
~~~~

Then finally, install the Tidal package:

~~~~bash
cabal install tidal
~~~~
