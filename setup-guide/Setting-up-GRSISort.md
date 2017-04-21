[**HOME**](Home) > [**GRSISORT SETUP GUIDE**](Setting-up-GRSISort)

Setting up GRSISort is a seven step process:

1. [Install git](#step1)
2. [Install Root](#step2)
3. [Download GRSISort](#step3)
4. [Setup Environment Variables](#step4)
5. [Compile GRSISort](#step5)
6. [???](#step6)
7. [Profit](#step7)

<a name="step1" />

## Step 1: Install git

To Install git, go to [GitHub](http://github.com) and click on *Download*.

Alternatively, Ubuntu users can use
```````
sudo apt-get install git
```````

<a name="step2" />

## Step 2: Install ROOT
*Already have ROOT? Proceed to [Download GRSISort](#step3)*

Before installing root make sure you have the [required and optional packages](http://root.cern.ch/drupal/content/build-prerequisites) installed. In particular, make sure you have a copy of GSL. For use of the [MathMore](https://root.cern.ch/mathmore-library), ROOT recommends GSL 1.8 or above, but some features have been observed to work with GSL 1.13. To identify your GSL version, type
```
gsl-config --version
```
in your terminal.

To download and install root:
Go to the [root webpage](https://root.cern.ch/downloading-root), select the version you want (we recommend using the a production version), and follow the instructions to download root.

Follow the [instructions to build root](https://root.cern.ch/building-root).

Finally, in your ./bashrc or ./profile, include the line
```bash
source /path/to/root/bin/thisroot.sh
```
The current supported version of root is v5.34.36. Here are some [Problems](Troubleshooting#rootproblems) that we have found at this stage.

<a name="step3" />

## Step 3: Download GRSISort

Verify that your root installation has the required features enabled. Check that the output of
```
root-config --features
```
contains cxx11, mathmore, python, and xml.

The newest version of GRSISort currently resides on GRIFFIN collaboration github repository.

To Clone (Download) the newest version of GRSISort, navigate to the directory where you want the GRSISort directory to be. Clone the repo using:
``````````
git clone https://github.com/GRIFFINCollaboration/GRSISort.git
``````````

GRSISort should now download to **./GRSISort**

Enter into the **GRSISort** directory with `cd GRSISort`.

To see a list of GRSISort versions available for download, use `git tag -l`. The current *stable* release will be tagged under `current`. The most up to date version will be tagged under `master`.

To download a the version named \<version\>:
```
git checkout <version>
```

*Don't like git? (You should) The newest version can also be downloaded from [[here|https://github.com/pcbend/GRSISort/tarball/master]]*.

If you want to contribute to the code, you need to have your own copy of GRSISort in your github account. To do this go to the GRSISort repository of the GRIFFINCollaboration and click "fork" in the top right. Now run
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

<a name="step4" />

## Step 4: Setup Environment Variables
The setup for GRSISort depends on the shell being used. To determine the Shell you are using execute
`````
echo $SHELL
`````
To setup GRSISort for a .sh shell (bash) execute `source thisgrsi.sh` in your terminal from the GRSISort Directory

To avoid running this script every session, add the following to your **~/.bash_profile** or **~/.profile**
```
source /Path/To/GRSISort/thisgrsi.sh
```
If you're using a c-type shell use the scripts with .csh extension instead of the .sh extension in the commands above.

<a name="step5" />

## Step 5: Compile GRSISort
To compile GRSISort type `make`. Please be patient as compilation will take a few moments. You can speed this up if you want by using `make -j` (uses all available cores) or `make -j N` where N is the number of jobs you want to run in parallel.

- To create the HTML documentation, you can compile with `make docs`. This is NOT REQUIRED.

If you receive an error, please consult the [Troubleshooting](troubleshooting) page.

<a name="step6" />

## Step 6: ???
Instructions unclear, lost in terminology.

<a name="step7" />

## Step 7: Profit
If you ever figure this out, please share the wealth.
