* [SWGEmu Server Packages](#sgwemu-server-packages)
    * [RPM](#rpm)
    * [Server Configuration](#server-configuration)
    * [FAQ](#faq)


# SWGEmu Server Packages

This repository provides the necessary files to build unofficial an RPM package for the Star Wars Galaxies Emulator (SWGEmu) server. This uses pinned versions of git commits from GitHub for the [Core3](https://github.com/swgemu/Core3) and [PublicEngine](https://github.com/swgemu/PublicEngine) projects. This RPM is currently **beta** quality. Use at your own risk.

Built RPMs are provided on the [ekultails/swgemu Fedora COPR](https://copr.fedorainfracloud.org/coprs/ekultails/swgemu/).

```
$ sudo dnf copr enable ekultails/swgemu
$ sudo dnf install swgemu-server
```


## RPM

Supported operating systems (x86_64):

* Fedora >= 28
* RHEL/CentOS 8

For building the RPM, optionally use a container. Note that sudo is not provided or required inside the container.

```
$ docker run -it --name swgemubuild fedora:28 bash
```

Install the dependencies to build the RPM, copy over the files required for the it, and then build it.

```
$ sudo dnf install 'dnf-command(builddep)' git rpm-build
$ git clone https://github.com/ekultails/swgemu-server-packages.git
$ mkdir -p ~/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}
$ cp -f -r swgemu-server-packages/rpm/SOURCES/* ~/rpmbuild/SOURCES/
$ cp -f -r swgemu-server-packages/rpm/SPECS/* ~/rpmbuild/SPECS/
$ sudo dnf builddep ~/rpmbuild/SPECS/swgemu-server.spec
$ rpmbuild -bb ~/rpmbuild/SPECS/swgemu-server.spec
```

The compiled RPM will be available at `~/rpmbuild/RPMS/x86_64/swgemu-server*.rpm`.


## Server Configuration

A SWGEmu server adminsitrator's guide is provided in the GitHub repository at [rpm/SOURCES/readme.md](rpm/SOURCES/readme.md). That documentation is provided in the RPM at `/opt/swgemu-server/doc/readme.md`.


## FAQ

* There is `XYZ` problem with SWGEmu. Where should issues be reported?
    * Only open GitHub issues here if there are issues building, installing, or updating the RPM itself. Visit [https://www.swgemu.com/bugs/](https://www.swgemu.com/bugs/) to view or open bug reports. Upstream patches can be submitted to [http://review.swgemu.com](http://review.swgemu.com).
* CentOS/RHEL 8 does not provide `lua-devel`. How can the RPM be installed?
    * Install `lua-devel` from the [ekultails/lua Fedora COPR](https://copr.fedorainfracloud.org/coprs/ekultails/lua/): `$ sudo dnf copr enable ekultails/lua && sudo dnf install lua-devel`
    * Or use the [CentOS 8 source code for `lua`](https://git.centos.org/rpms/lua/tree/c8) to build all of the relevant RPMs following these [instructions](https://wiki.centos.org/Sources#head-8b5a127334c95d7340a4952ab9622a83988076c0). This will build the required `lua-devel` RPM to help fulfill all of the build dependencies.
* Why are there no Debian packages?
    * There is already an official SWGEmu development virtual machine called [ZonamaDev](https://github.com/Zonama/ZonamaDev). This uses Debian as the operating system. There is also support for deploying SWGEmu to [Debian docker containers on Kubernetes](https://github.com/TheAnswer/Core3/commit/5815f8f975f899f626bf39e8283ae1040f087db7).


# License

This software is licensed under the GPLv3. More information about this can be found in the included "LICENSE" file or online at: http://www.gnu.org/licenses/gpl-3.0.en.html
