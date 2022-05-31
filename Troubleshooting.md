[**HOME**](Home) > [**TROUBLESHOOTING**](TroubleShooting)

1. [Unfixable Problems](#unfixable)
  - [Architecture](#unfixableArch)
2. [GRSI Installation](#installation)  
3. [HTML Documentation](#fixablehtml)
4. [ROOT Installation](#rootproblems)
5. [Error Report Template](errorreport)

<a name="unfixable" />

# Unfixable Problems

<a name="unfixableArch" />

### Architecture

- Only 64-bit architectures are currently supported. The analysis side of GRSISort may eventually be expanded to 32-bit machines ... but why not have an excuse to upgrade?

---
<a name="fixableOSX" />

# GRSI Installation

### Installing under OSX

For Mac OS X you need to have greadlink and gfind installed. They can be installed using mac ports (https://www.macports.org) via ```port install coreutils findutils```, or using home brew (https://brew.sh) using ```brew install coreutils findutils```.

More instructions to follow ...

<a name="fixablehtml" />

### HTML
- If you receive the error `sh: 1: dot: not found` it is because you do not have a **dot** writing program. This is not really an issue, but to avoid this, install a dot program like **graphviz**. (On Ubuntu `sudo apt-get install graphviz`)

## ROOT
<a name="rootproblems" />

#### freetype
- You may receive the error in **asfont.c** that **freetype/freetype.h** does not exist. This can be solved by changing *line 67* in **asfont.c** from
````c++
CODE: SELECT ALL
#include <freetype/freetype.h>
````
to
````c++
CODE: SELECT ALL
#include <freetype.h>
````

#### Standard C Libraries
- If you receive the errors:
```bash
Error: cannot open file "gnu/stubs.h" /usr/include/features.h:399:
Error: cannot open file "bits/wchar.h" /usr/include/stdint.h:27:
Error: cannot open file "bits/wordsize.h" /usr/include/stdint.h:28:
```
You're missing the 32 bit libc dev package:

On **Ubuntu** it's called `libc6-dev-i386` - do `sudo apt-get install libc6-dev-i386`. See below for extra instructions for Ubuntu 12.04.

On **Red Hat** distros, the package name is `glibc-devel`

On **CentOS 5.8**, the package name is `glibc-devel`

On **CentOS 6 / 7**, the package name is `glibc-devel`.

On **SLES** it's called `glibc-devel-32bit` - do `zypper in glibc-devel-32bit`

Are you using Ubuntu 12.04? There is a known problem that puts the files in a non standard location. You'll also need to do:
```bash
export LIBRARY_PATH=/usr/lib/$(gcc -print-multiarch)
export C_INCLUDE_PATH=/usr/include/$(gcc -print-multiarch)
export CPLUS_INCLUDE_PATH=/usr/include/$(gcc -print-multiarch)
```

If you encounter the problem:
```bash
.... /usr/bin/ld: cannot find -lstdc++ ....
```

On **Ubuntu** you will need to do  `sudo apt-get install g++-multilib`

On **CentOS 5** you will need to do `yum install libstdc++-devel`

On **CentOS 6** you will need to do `yum install libstdc++-devel`

#### XML
- If you have any problems with XML, etc. Please make sure you followed the ROOT download instructions very carefully including the installation of packages such as `libxml2-dev`.

-------------------

If you have an issue that is currently not on the above list please post it to the [Issue Tracker](https://github.com/GRIFFINCollaboration/GRSISort/issues) after filling out an [Error Report](errorreport).
