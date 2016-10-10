# linuxdeployqt [![Build Status](https://travis-ci.org/probonopd/linuxdeployqt.svg?branch=master)](https://travis-ci.org/probonopd/linuxdeployqt) [![discourse](https://img.shields.io/badge/forum-discourse-orange.svg)](http://discourse.appimage.org/t/linuxdeployqt-new-linux-deployment-tool-for-qt/57) [![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/probonopd/AppImageKit?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge) [![irc](https://img.shields.io/badge/IRC-%23AppImage%20on%20freenode-blue.svg)](https://webchat.freenode.net/?channels=AppImage)

This Linux Deployment Tool for Qt, `linuxdeployqt`, takes an application as input and makes it self-contained by copying in the Qt libraries and plugins that the application uses into a bundle. This can optionally be put into an [AppImage](http://appimage.org/).

## Differences to macdeployqt
This tool is conceptually based on the [Mac Deployment Tool](http://doc.qt.io/qt-5/osx-deployment.html), `macdeployqt` in the tools applications of the Qt Toolkit, but has been changed to a slightly different logic and other tools needed for Linux.

* Instead of an `.app` bundle for macOS, this produces an [AppDir](http://rox.sourceforge.net/desktop/AppDirs.html) for Linux
* Instead of a `.dmg` disk image for macOS, this produces an [AppImage](http://appimage.org/) for Linux which is quite similar to a dmg but executes the contained application rather than just opening a window on the desktop from where the application can be launched

## Known issues

__This may not be fully working yet.__ Use with care, run with maximum verbosity, submit issues and pull requests. Help is appreciated

## Installation

* If you believe in dogfeeding, download __linuxdeployqt-x86_64.AppImage__ from the [Releases](https://github.com/probonopd/linuxdeployqt/releases) page and `chmod a+x` it. Then you can skip the following steps because it is a self-contained bundle of linuxdeployqt (made with linuxdeployqt of course)

* Get and build linuxdeployqt e.g., using Qt 5.7.0 (you could use this [Qt Creator AppImage](https://bintray.com/probono/AppImages/QtCreator#files) for this)

```
sudo apt-get -y install git g++ libgl1-mesa-dev
git clone https://github.com/probonopd/linuxdeployqt.git
```

* Build and install [patchelf](https://nixos.org/patchelf.html) (a small utility to modify the dynamic linker and RPATH of ELF executables; similar to `install_name_tool` on macOS). To learn more about this, see http://blog.qt.io/blog/2011/10/28/rpath-and-runpath/

```
wget https://nixos.org/releases/patchelf/patchelf-0.9/patchelf-0.9.tar.bz2
tar xf patchelf-0.9.tar.bz2
( cd patchelf-0.9/ && ./configure  && make && sudo make install )
```

* Optional if you want to generate AppImages: Download [AppImageAssistant](https://github.com/probonopd/AppImageKit/releases) and put it into your $PATH, e.g., into `/usr/local/bin`. Make sure it is renamed to `AppImageAssistant` and is `chmod a+x`

```
wget https://github.com/probonopd/AppImageKit/releases/download/6/AppImageAssistant_6-x86_64.AppImage -O AppImageAssistant
sudo mv AppImageAssistant /usr/local/bin/
```

## Usage

Open in Qt Creator and build your application. Run it from the command line and inspect it with `ldd` to make sure the correct libraries from the correct locations are getting loaded, as `linuxdeployqt` will use `ldd` internally to determine from where to copy libraries into the bundle.

__Important:__ `linuxdeployqt` deploys the Qt instance that qmake on the $PATH points to, so make sure that it is the correct one. Verify that qmake finds the correct Qt instance like this before running the `linuxdeployqt` tool:

```
qmake -v

QMake version 3.0
Using Qt version 5.7.0 in /tmp/.mount_QtCreator-5.7.0-x86_64/5.7/gcc_64/lib
```
If this does not show the correct path to your Qt instance that you want to be bundled, then adjust your `$PATH` to find the correct `qmake`.

```
Usage: linuxdeployqt app-binary [options]

Options:
   -verbose=<0-3>      : 0 = no output, 1 = error/warning (default), 2 = normal, 3 = debug
   -no-plugins         : Skip plugin deployment
   -appimage           : Create an AppImage
   -no-strip           : Don't run 'strip' on the binaries
   -bundle-non-qt-libs : Also bundle non-core, non-Qt libraries
   -executable=<path>  : Let the given executable use the deployed libraries too
   -qmldir=<path>      : Scan for QML imports in the given path
   -always-overwrite   : Copy files even if the target file exists
   -libpath=<path>     : Add the given path to the library search path

linuxdeployqt takes an application as input and makes it
self-contained by copying in the Qt libraries and plugins that
the application uses.
```

## Contributing

These are my first steps with Qt and with C++ for that matter, and it is stil very young, so I'd appreciate your testing, comments, and (ideally) code review. Please discuss in the [forum](http://discourse.appimage.org/t/linuxdeployqt-new-linux-deployment-tool-for-qt/57) or using GitHub issues and pull requests.
