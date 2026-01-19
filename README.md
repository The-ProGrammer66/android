i-scream-sandwich: Ice Cream Sandwich Restoration Project
=========================================================

This repository contains reconstructed `repo` manifests of pre-release Android 4.0 ("Ice Cream Sandwich") builds.

As of now, the following builds have been reconstructed:

| Build ID & manifest branch               | Status              |
| :--------------------------------------: | :-----------------: |
| [`IRJ20`]  (April 20th, 2011)            | Done                |
| [`IRJ32`]  (May 2nd, 2011)               | Done                |
| [`IRJ40`]  (May 10th, 2011)              | Done                |
| [`IRJ60`]  (May 30th, 2011)              | Done                |
| [`IRJ81`]  (June 20th, 2011)             | Done                |
| [`IRK04`]  (July 4th, 2011)              | Done                |
| [`IRK22`] (July 22th, 2011)              | Done                |
| [`IRK36B`] (August 6th, 2011)            | Done                |
| [`IRK40C`] (August 9th, 2011)            | Done                |  
| [`IRK48`] (August 17th, 2011)            | Done                |
| [`IRK54B`] (August 23rd, 2011)           | Done                | 
| [`IRK61`] (August 30th, 2011)            | Done                |
| [`IRK69`] (September 7th, 2011)          | Done                |
| [`IRK77`] (September 15th, 2011)         | Done                |
| [`IRK88B`] (September 26th, 2011)        | Done                |
| [`IFL10`] (October 10th, 2011)           | Done                |
| [`ITL28B`] (October 28th, 2011)          | Done                |

[`IRJ20`]:  https://github.com/froyocomb/android/blob/i-scream-sandwich/IRJ20.xml
[`IRJ32`]:  https://github.com/froyocomb/android/blob/i-scream-sandwich/IRJ32.xml
[`IRJ40`]:  https://github.com/froyocomb/android/blob/i-scream-sandwich/IRJ40.xml
[`IRJ60`]:  https://github.com/froyocomb/android/blob/i-scream-sandwich/IRJ60.xml
[`IRJ81`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRJ81.xml
[`IRK04`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK04.xml
[`IRK22`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK22.xml
[`IRK36B`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK36B.xml
[`IRK40C`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK40C.xml
[`IRK48`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK48.xml
[`IRK54B`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK54B.xml
[`IRK61`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK61.xml
[`IRK69`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK69.xml
[`IRK77`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK77.xml
[`IRK88B`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IRK88B.xml
[`IFL10`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/IFL10.xml
[`ITL28B`]: https://github.com/froyocomb/android/blob/i-scream-sandwich/ITL28B.xml

Preparing a Build Environment
-----------------
**All of this must only be done once! Once a proper installation is set up, builds can be switched using just `repo init`. Make sure to erase the previous build's folders and files (with the exception of the hidden `.repo` folder, do not remove it) before changing.**

It is recommended to use an older Linux distribution. All builds have been tested on Ubuntu 12.04 ("Precise Pangolin"), which can be downloaded from [here](https://old-releases.ubuntu.com/releases/12.04/ubuntu-12.04.4-desktop-amd64.iso). Do not use anything newer, like Ubuntu 14.04.

Once Ubuntu 12.04 LTS is installed, as it is now a legacy edition of Ubuntu, the default repositories have to be changed to "old-releases.ubuntu.com". To do this, open Terminal and type in `sudo gedit /etc/apt/sources.list`. Change all references of the URLs from e.g. `http://archive.ubuntu.com` to `http://old-releases.ubuntu.com`. Do the same for `http://security.ubuntu.com`, but don't touch the two bottom-most URLs. Make sure to only change the beginning of the aforementioned URLs.

If running in a virtual machine, install VMware Tools now, or else its features won't work properly (press enter on all questions given by `./vmware-install.pl`), then reboot.

Update the machine:
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install duplicity linux-headers-generic-lts-saucy linux-image-generic-lts-saucy ubuntu-minimal
sudo reboot
```
You can now proceed with the rest of the guide.

**Prerequisites**
------------------
First, install all the needed dependencies needed for building AOSP:

```
sudo apt-get install git gnupg flex bison gperf build-essential zip curl g++-4.6-multilib gcc-4.4 g++-4.4 gcc-4.4-multilib g++-4.4-multilib libc6-dev libncurses5-dev:i386 x11proto-core-dev libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 libglapi-mesa libgl1-mesa-dev mingw32 openjdk-7-jdk tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386
```

Additionally run:
```
sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so
sudo apt-get autoremove
```

Ice Cream Sandwich builds require Sun Java 1.6. The package for this is named `jdk-6u45-linux-x64.bin` - mirrors or the official download can be found by searching it up.

After acquiring the package, run the following (this assumes the package is on the Desktop and the terminal is on the same path where the file is):
```
chmod +x jdk-6u45-linux-x64.bin
./jdk-6u45-linux-x64.bin
sudo mkdir -p /usr/lib/jvm/jdk1.6.0_45
sudo mv jdk1.6.0_45/* -f /usr/lib/jvm/jdk1.6.0_45/
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.6.0_45/bin/java" 1
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.6.0_45/bin/javac" 1
sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.6.0_45/bin/javaws" 1
```

Once done, run `sudo update-alternatives --config java` and switch the system over to `jdk1.6.0_45`. Do the same for `javac` and `javaws`, by replacing `java` with `javac` or `javaws`. `javaws` will likely not have an alternative, if so, skip it.

Install newer `git`:
```
sudo apt-add-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get upgrade
```

To get modern `repo` working, install Python 3.6:

```
sudo apt-get build-dep python3.2
wget https://www.python.org/ftp/python/3.6.15/Python-3.6.15.tgz
tar -xvf Python-3.6.15.tgz && cd Python-3.6.15
sudo ./configure --enable-optimizations
sudo make -j6 && sudo make install
```

I would also recommend setting a root password because of the next step.

If you somehow manage to screw up the X11 install on 12.04 LTS (for example being stuck on the boot screen with all 5 dots lit orange), which is for some reason super common, access a terminal (via any method, e.g. switching TTY modes, recovery mode...), then run `sudo apt-get install --reinstall xserver-xorg`, and reboot.
 
Downloading Source
------------------

To get started with downloading the source code, experience with Git and [`repo`](https://source.android.com/docs/setup/reference/repo) is needed.

To set up repo, do the following:
```
mkdir -p ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

Run `gedit ~/.bashrc`, navigate to the bottom and paste in the line `export PATH=${PATH}:~/bin`. Restart the terminal and then create the `android/system` folders in the directory you wish to download and compile Android in. Navigate to the `system` folder and then proceed with the rest of the guide.

To initialize a repository tree using one of the manifests provided by this project, execute a command like this (see the table above for available `<build>`s):

    repo init -u https://github.com/froyocomb/android.git -b i-scream-sandwich -m <build>.xml --depth=1

If `git` asks you to set your email and username, do so appropiately. Your actual username and email don't have to be used of course, just anything.

Then to download the respective code, execute:

    repo sync --no-tags --no-clone-bundle -c

Compiling
---------

To initialize the build environment, execute the following command:

 ```
. build/envsetup.sh
 ```

Then pick from one of the available build targets by executing the command:

    lunch

To compile Android, type:

    make CC=gcc-4.4 CXX=g++-4.4 -j$(nproc)

Please make sure to remember that certain builds may require exclusive patches that are mentioned on their respective BetaWiki page. If you face any issues during the compile, ask in the Discord server or on the "Issues" page on GitHub.

Running
-------

You can run the compiled build with the Android Emulator, although it is recommended to simply use real hardware.

In the Ubuntu build environment, you may run the currently compiled build with the in-tree emulator by executing the command:

    emulator

To set a custom resolution, type `-skin 1920x1080`, replacing 1920x1080 with your desired resolution.

Useful Links
------------

* GitHub search filter for non-SVN (i.e. Android) commits from the LineageOS LLVM & Clang mirrors, ordered by commit date
  * [LLVM](https://github.com/search?q=repo%3ALineageOS%2Fandroid_external_llvm+NOT+%22git-svn-id%3A%22&type=commits&s=committer-date&o=asc)
  * [Clang](https://github.com/search?q=repo%3ALineageOS%2Fandroid_external_clang+NOT+%22git-svn-id%3A%22&type=commits&s=committer-date&o=asc)
* [Every IR-series build from before 4.0.1 r1 was tagged](https://android.googlesource.com/platform/build/+log/598288e321cc4cec399afb20ff6485bf3b8ac953)
* [Froyocomb Helper](https://gist.github.com/Dobby233Liu/c55c1e9c816facd153eeb19e386f53fd): userscript to assist finding commits before a certain time 
