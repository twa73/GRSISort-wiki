[**HOME**](Home) > [**TROUBLESHOOTING**](TroubleShooting)

1. [Unfixable Problems](#unfixable)
  - [Operating Systems](#unfixableOS)
  - [Architecture](#unfixableArch)
2. [Fixable Problems](#fixable)  
  - [HTML Documentation](#fixablehtml)
  - [ROOT Installation](#rootproblems)

<a name="unfixable" />
------------------
#UnFixable Problems
<a name="unfixableOS" />
###Operating System 
- MacOSX is currently not supported as the current version of ROOT (v5.34) uses the wrong libraries (Clang Based). This problem will hopefully be resolved in ROOT v6.00.

<a name="unfiaxableArch" />
###Architecture
- Only 64-bit architectures are currently supported. The analysis side of GRSISort may eventually be expanded to 32-bit machines...but why not have an excuse to upgrade?

------------------
<a name="fixable" />
#Fixable Problems

<a name="fixablehtml" />
###HTML
- If you recieve the error `sh: 1: dot: not found` it is because you do not have a **dot** writing program. This is not really an issue, but to avoid this, install a dot program like **graphviz**. (On Ubuntu `sudo apt-get install graphviz`) 

##ROOT
<a name="rootproblems" />
####freetype
- You may recieve the error in **asfont.c** that **freetype/freetype.h** does not exist. This can be solved by changing *line 67* in **asfont.c** from
````c++
CODE: SELECT ALL
#include <freetype/freetype.h>
````
to
````c++
CODE: SELECT ALL
#include <freetype.h>
```

####stdlibc
- If you recieve the errors:
```bash
Error: cannot open file "gnu/stubs.h" /usr/include/features.h:399:
Error: cannot open file "bits/wchar.h" /usr/include/stdint.h:27:
Error: cannot open file "bits/wordsize.h" /usr/include/stdint.h:28:
```
Install the `stdlibc-dev` package (`sudo apt-get install stdlibc-dev-i386` on Ubuntu).

####XML
- If you have any problems with XML, etc. Please make sure you followed the ROOT download instructions very carefully including the installation of packages such as `libxml2-dev`.

-------------------

If you have an issue that is currently not on the above list please post it to the [Issue Tracker](https://github.com/pcbend/GRSISort/issues).
