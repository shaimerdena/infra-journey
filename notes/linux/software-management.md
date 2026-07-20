# Software Management

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [Software Management](#software-management)
  - [Understanding RPM and DEB Software Packaging](#understanding-rpm-and-deb-software-packaging)

## Understanding RPM and DEB Software Packaging

**RPM (Red Hat Package Manager)**
- RPM is a package management system used by Red Hat-based distributions (e.g., RHEL, CentOS, Fedora).
- RPM packages have the `.rpm` file extension. Later `.yum` was introduced for managing RPM packages, which was later replaced by `dnf` in newer versions of Fedora and RHEL.
- RPM provides tools for installing, upgrading, and removing software packages.

**DEB (Debian Package)**
- DEB is the package format used by Debian-based distributions (e.g., Ubuntu).
- DEB packages have the `.deb` file extension.
- DEB provides similar functionality to RPM but is tailored for Debian's package management system.

**Key Differences**
- Package Management Tools:
  - RPM uses `rpm` and `dnf`/`yum` for package management.
  - DEB uses `dpkg` and `apt` for package management.
- Dependency Resolution:
  - RPM relies on the user to resolve dependencies.
  - DEB automatically resolves dependencies using APT.