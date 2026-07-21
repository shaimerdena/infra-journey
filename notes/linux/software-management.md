# Software Management

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [Software Management](#software-management)
  - [Understanding RPM and DEB Software Packaging](#understanding-rpm-and-deb-software-packaging)
    - [DEB packaging](#deb-packaging)
    - [RPM packaging](#rpm-packaging)

## Understanding RPM and DEB Software Packaging
  
### DEB packaging

**DEB (Debian Package)**
- DEB is the package format used by Debian-based distributions (e.g., Ubuntu).
- DEB packages have the `.deb` file extension.
- DEB uses `dpkg` and `apt` for package management.
- DEB provides similar functionality to RPM but is tailored for Debian's package management system.
- DEB automatically resolves dependencies using APT.

Debian sofware packages hold multiple files and metadata, including: Control files (metadata, located in the `DEBIAN` directory), Data files (actual files that will be installed on the system), Scripts (include pre-installation and post-installation scripts), Configuration files (placed in `/etc`), Documentation (such as README files, changelogs, and license information, placed in `/usr/share/doc`).

Multplie command-line tools are available for managing DEB packages, including:
- Ubuntu Software Center: A graphical interface for managing DEB packages on Ubuntu.
- `aptitude`: A text-based interface for managing DEB packages, providing a more user-friendly experience than the command line. You can upgrade
packages, get new packages, or view installed packages.
- `apt` (apt-get, apt, apt-config, apt-cache, and so on): set of commands that can be used to manage package installation.

| Command | Description |
| --- | --- |
| sudo apt-get update | Get the latest package versions |
| sudo apt-cache search SOFTWARE | Find package by key word |
| sudo apt-cache show SOFTWARE | Display information about a package |
| sudo apt-get install SOFTWARE | Install the software package |
| sudo apt-get upgrade | Update installed packages if upgrade ready |
| sudo apt-cache pkgnames | List all packages that are installed |

### RPM packaging

**RPM (Red Hat Package Manager)**
- RPM is a package management system used by Red Hat-based distributions (e.g., RHEL, CentOS, Fedora).
- RPM packages have the `.rpm` file extension. Later `.yum` was introduced for managing RPM packages, which was later replaced by `dnf` in newer versions of Fedora and RHEL.
- RPM provides tools for installing, upgrading, and removing software packages.
- RPM relies on the user to resolve dependencies.

An **RPM (Red Hat Package Manager)** package is a file that contains everything needed to install software on RPM-based Linux distributions (Fedora, RHEL, Rocky Linux, AlmaLinux). An RPM package may include: Program files (executables), Configuration files, Documentation, Metadata.

---

**RPM Package Name & Query Installed Packages**

Example:

```text
$ sudo rpm -­q firefox
firefox-141.0-2.fc42.x86_64.rpm
```

Meaning:

| Part | Description |
|------|-------------|
| firefox | Package name |
| 141.0 | Software version (from Mozilla) |
| 2 | Package release number (assigned by Fedora) |
| fc42 | Built for Fedora 42 |
| x86_64 | 64-bit architecture |
| .rpm | RPM package file |

---

**RPM Database**

After installation, package information is stored in the **local RPM database**. The database keeps information about: Installed packages, Versions, Dependencies, Installation details

---

**Show Package Information**

Display detailed information:

```bash
rpm -qi firefox
```
---