[**HOME**](Home) > [**GRSISORT SETUP GUIDE**](Setting-up-GRSISort)

Setting up GRSISort is a seven step process:

1. [Install git](#step1)
2. [Install Root](#step2)
3. [Download GRSISort](#step3)
4. [Compiling GRSISort and data parser/detector libraries](#step4)
5. [Setup Environment Variables](#step5)

<a name="step1" />

## Step 1: Install git

To Install git, go to [GitHub](http://github.com) and click on *Download*.

Alternatively, Ubuntu users can use
```````
sudo apt install git
```````

<a name="step2" />

## Step 2: Install ROOT
*Already have ROOT? Proceed to [Download GRSISort](#step3)*

Before installing root make sure you have the [required and optional packages](https://root.cern/install/dependencies) installed. In particular, make sure you have a copy of GSL. For use of the [MathMore](https://root.cern.ch/mathmore-library), ROOT recommends GSL 1.8 or above, but some features have been observed to work with GSL 1.13. To identify your GSL version, type
```
gsl-config --version
```
in your terminal.

Go to the [root webpage](https://root.cern.ch/downloading-root), select the version you want (we recommend using a production version, i.e. with an even minor number), and follow the instructions to download root.

Follow the [instructions to build root](https://root.cern.ch/building-root).

Finally, in your ./bashrc or ./profile, include the line
```bash
source /path/to/root/bin/thisroot.sh
```
The current supported versions of root are the stable 6er versions. Root 5 is no longer supported. You might be able to run the code with v5.34.36, but we won't be able to guarantee that nor help with problems. Here are some [problems](Troubleshooting#rootproblems) that we have found at this stage.

<a name="step3" />

## Step 3: Download GRSISort

Verify that your root installation has the required features enabled. Check that the output of
```
root-config --features
```
contains mathmore, python, and xml (if xml is missing you can still compile GRSISort, but some feature will be missing).

The newest version of GRSISort currently resides on GRIFFIN collaboration github repository.

To Clone (Download) the newest version of GRSISort, navigate to the directory where you want the GRSISort directory to be. Clone the repo using:
``````````
git clone https://github.com/GRIFFINCollaboration/GRSISort.git
``````````

GRSISort should now download to **./GRSISort**

Enter into the **GRSISort** directory with `cd GRSISort`.

Optional:
*
To see a list of GRSISort versions available for download, use `git tag -l`.
The current *stable* release will be tagged under `current`.
The most up to date version will be tagged under `main`, see [releases](https://github.com/GRIFFINCollaboration/GRSISort/releases) for details.

To download the version named \<version\>:
```
git checkout <version>
```

### Contributing to the code

If you want to contribute to the code (which is very much encouraged!), you need to have your own copy of GRSISort in your github account. To do this go to the GRSISort repository of the GRIFFINCollaboration and click "fork" in the top right. Now run
```
git remote set-url origin ssh://git@github.com/<UserName>/GRSISort.git
git remote add upstream https://github.com/GRIFFINCollaboration/GRSISort.git
```
where <UserName> is your own user name on github.com.

To verify this worked, you can run
```
git remote -v
```
which should give you this output (again with <UserName> replaced by your own user name on github.com):
```
origin	ssh://git@github.com/<UserName>/GRSISort.git (fetch)
origin	ssh://git@github.com/<UserName>/GRSISort.git (push)
upstream	https://github.com/GRIFFINCollaboration/GRSISort.git (fetch)
upstream	https://github.com/GRIFFINCollaboration/GRSISort.git (push)
```

For this (using the ssh authentication) to work you need to set up SSH keys:
  - Go to your profile->Settings.
  - Select "SSH and GPG keys" on the left.
  - Add you SSH key (after following the instructions under "generating SSH keys" if needed).

<a name="step4" />

## Step 4: Compiling GRSISort and data parser/detector libraries

### Using cmake (recommended)

There are different ways to compile GRSISort with cmake.
All of them involve two steps: creating and configuring a build directory, and compiling GRSISort in this build directory.
The differences are whether you use cmake to create the build directory or do it by hand, and whether you use cmake's build interface to the native build tool or use the build tool directly.

The configuration step includes the download of all data parser and detector libraries.
During compilation only the requested data library (or none) will be built if you use a cmake version 3.28 or newer.

So far four parsing/detector libraries have been written:

- GRSIData for GRIFFIN/TIGRESS experiments (https://github.com/GRIFFINCollaboration/GRSIData)
- ILLData for FIPPS data from ILL (https://github.com/UoG-Nuclear-Physics-Group/ILLData)
- iThembaData for data from the tape station at iThemba (https://github.com/UoG-Nuclear-Physics-Group/iThembaData)
- HILData for data from HIL, Warsaw (https://github.com/UoG-Nuclear-Physics-Group/HILData.git)

To configure and compile from outside the build directory (one level up from the GRSISort directory):
```
cmake -S GRSISort -B GRSISort-build
cmake --build GRSISort-build --target libraryName
```
Where libraryName is one of GRSIData, HILData, ILLData, or iThembaData.
If you do not need a data parser and detector library (or you are using a cmake version older than 3.28), you can leave out the `--target` flag completely.

If you compiled a library you can either provide the location of the libraries .so file (typically `$(GRSISYS)/lib/lib<libraryName>.so`) on the command line, or edit .grsirc to include a line
```
GRSI.ParserLibrary: <path to library, e.g. $(GRSISYS)/lib/libGRSIData.so>
```

*Note*: on macOS you might need to add a flag to tell cmake where to find ROOT (e.g. `-DCMAKE_PREFIX_PATH=/opt/local/libexec/`).

### Using the old method with make

To compile GRSISort type `make` from within the GRSISort directory.
Please be patient as compilation will take a few moments.
You can speed this up if you want by using `make -j` (uses all available cores) or `make -j N` where N is the number of jobs you want to run in parallel.

- To create the HTML documentation, you can compile with `make docs`. This is NOT REQUIRED.

If you receive an error, please consult the [Troubleshooting](troubleshooting) page.

In order to parse data and to understand the setup specific detector classes, an additional library is needed.
This library has to provide the following functions:
- CreateFile(std::string& filename) which returns a pointer to a new raw data file and DestroyFile which takes a pointer to a raw data file and deletes it (returns nothing, i.e void).
- CreateParser() which creates a new data parser object and DestroyParser which takes a pointer to such a data parser object and deletes it.
- LibraryVersion() which returns a string containing the version number of the library.
- InitLibrary() which initializes e.g. the mnemonic class used in Channel.

So far four parsing/detector libraries have been written:

- GRSIData for GRIFFIN/TIGRESS experiments (https://github.com/GRIFFINCollaboration/GRSIData)
- ILLData for FIPPS data from ILL (https://github.com/UoG-Nuclear-Physics-Group/ILLData)
- iThembaData for data from the tape station at iThemba (https://github.com/UoG-Nuclear-Physics-Group/iThembaData)
- HILData for data from HIL, Warsaw (https://github.com/UoG-Nuclear-Physics-Group/HILData.git)

Any of those four libraries can be downloaded in the same manner as GRSISort using ```git clone <url>```. 
This should be done from within the GRSISort directory.

To use a library you can either provide the location of the libraries .so file on the command line, or edit .grsirc to include a line
```
GRSI.ParserLibrary: <path to library, e.g. $(GRSISYS)/GRSIData/lib/libGRSIData.so>
```

In order to compile the library you typically just call ```make <library-name>``` from the GRSISort directory or ```make``` in the directory of the library (plus any options you want to pass to make).
If this does not work please follow the instructions of the library you chose to download.
Multiple libraries can be installed in parallel.

<a name="step5" />

## Step 5: Setup Environment Variables
The setup for GRSISort depends on the shell being used. To determine the Shell you are using execute
`````
echo $SHELL
`````
To setup GRSISort for a .sh shell (bash) execute `source thisgrsi.sh` in your terminal from the GRSISort Directory (*the build directory if using cmake*).

To avoid running this script every session, add the following to your **~/.bash_profile** or **~/.profile**
```
source /Path/To/GRSISort/thisgrsi.sh
```
If you're using a c-type shell use the scripts with .csh extension instead of the .sh extension in the commands above.

