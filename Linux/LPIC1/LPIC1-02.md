# Managing Software

- [Managing Software](#managing-software)
  - [RPM packages](#rpm-packages)
    - [RPM Distributions and Conventions](#rpm-distributions-and-conventions)
    - [The rpm Command Set](#the-rpm-command-set)
    - [Extracting Data from RPMs](#extracting-data-from-rpms)
  - [Debian packages](#debian-packages)

## RPM packages

### RPM Distributions and Conventions

The convention for naming RPM packages is as follows:

```bash
packagename-a.b.c-x.arch.rpm
```

- **Package name**: is the name of the package.
  > such as samba.

- **Version number**: `a.b.c` is the package version number.
  > such as 2.2.7a.

- **Build number**: `x` is the build number (also known as the *release number*).
  > This number represents minor changes made by the *package maintainer*, **not** by the *program author*.

- **Architecture**: `arch` is a code for the package’s architecture.
  > i386 architecture represents a file compiled for any x86 CPU.  
  > x86_64 for the AMD64 platform.  
  > Scripts, documentation, and other CPU-independent packages generally use the **noarch** architecture code.  
  > Source RPMs, which use the **src** architecture code.

### The rpm Command Set

```bash
$ rpm [operation][options] [package-files|package-names]

rpm -i # Installs a package; system must not contain a package of the same name.
rpm -U # Installs a new package or upgrades an existing one.
rpm -V # or -y or --verify: Verifies a package
rpm -e # Uninstalls a package
rpm -q # Queries a package—finds if a package is installed, what files it con- tains, and so on.
-a or --all # Queries or verifies all packages.
--force # Forces installation of a package even when it means overwriting existing files or packages.
--nodeps # Specifies that no dependency checks be performed.
```

examples:

```bash
rpm -ivh --force # Use this for test your own rpm packages
rpm -Uvh samba-3.0.10-1.fc3.i386.rpm #  Upgrade a package
rpm -qa # query all packages
rpm -qi # displays information such as when and on what computer the binary package was built
```

### Extracting Data from RPMs

A good way to retrieve the original source code from a source RPM for compiling the software.

```bash
rpm2cpio X.rpm > X.cpio # outputs the cpio archive

# -i extract an archive and --make-directories to create directories:
cpio -i --make-directories < X.cpio

# OR

rpm2cpio X.rpm | cpio -i --make-directories
```

## Debian packages
