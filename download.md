---
layout: default
title: Download - Reinteract
page: download
---

<h2><a id="Windows">Windows</a></h2>

[Reinteract installer for Python 2.7](http://www.reinteract.org/download/windows/Reinteract-0.5.9-python2.7.msi). (16MB download, 50MB installed.)

Requires [Python 2.7 distribution](http://www.python.org/ftp/python/2.7.2/python-2.7.2.msi) from [python.org](http://python.org/download/).

The installer includes everything that you need to run Reinteract other than Python: the GTK+ libraries, the Python bindings for the GTK+ libraries, Numpy, and matplotlib. If you already have some of these installed and know that you would rather use the system instead, you can do a custom install and skip those components (advanced users only.)

[Installer sources and other versions](http://www.reinteract.org/download/windows/). [How the installer was created](http://git.fishsoup.net/cgit/reinteract/tree/tools/build_msi/README).

<h2><a id="MacOSX">Mac OS X</a></h2>

[Reinteract for Intel and PPC, OS X 10.4 or later](http://www.reinteract.org/download/osx/Reinteract-0.5.9.dmg). (22MB download, 80MB installed.) To install, double click on the disk image to open it and drag Reinteract to your Applications folder.

Requires Python 2.6 or newer. On OS X 10.4 (Tiger) and 10.5 (Leopard) , you'll need to download and install the Python 2.7 distribution from [python.org](http://python.org/download).

[Installer sources and other versions](http://www.reinteract.org/download/osx/). [How the installer was created](http://git.fishsoup.net/cgit/reinteract/tree/tools/build_bundle/README). [Instructions for building from source](http://git.fishsoup.net/cgit/reinteract/tree/tools/build_deps_osx/README)

<h2><a id="Linux">Linux</a></h2>

Reinteract is available through the package repositories for Fedora and other major Linux distributions. It's also very easy to get the latest version of Reinteract from source control.

<h2><a id="Source">Source</a></h2>

The software can be obtained via the Git source control system as:

```
git clone git://git.fishsoup.net/reinteract
```

[Browse the repository](http://git.fishsoup.net/cgit/reinteract/tree/) [Recent changes](http://git.fishsoup.net/cgit/reinteract/log/)

Run it:

    bin/uninst.py

To set it up to run from the command line on a Linux system:

    mkdir -p ~/bin
    ln -s `pwd`/bin/uninst.py ~/bin/reinteract

[Sources releases](http://www.reinteract.org/download/sources/) are useful only if you are creating a package to distribute for your operating system.
