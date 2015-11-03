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
##Step 2: Install ROOT
Before installing root make sure you have the [required and optional packages](http://root.cern.ch/drupal/content/build-prerequisites) installed.

To download and install root:
First, clone the root git repository where you want root on your computer:
`````
git clone https://github.com/root-mirror/root.git
`````
Move into the ROOT directory using
```
cd root
```
Then, choose the version of root you want. To see the list of ROOT versions, type:
`````
git tag -l
`````
To choose a version:
`````
git checkout -b version_name version_name
`````
where version name is one of the versions printed in the previous command. Configure ROOT with 
``` 
./configure
make ./config --options
```
For a list of options, and more detail, see the [ROOT User manual](https://root.cern.ch/root/htmldoc/guides/users-guide/ROOTUsersGuide.html#installing-root). Compile ROOT using 
```
make
```
Finally, in your ./bashrc or ./profile, include the line
```bash
source /path/to/root/bin/thisroot.sh
```
The current supported version of root is v5.34.24, Here are some [Problems](Troubleshooting#rootproblems) that we have found at this stage.

*Already have ROOT? Proceed to [Download GRSISort](#step3)*

<a name="step3" />
## Step 3: Download GRSISort

The newest version of GRSISort currently resides on GRIFFIN collaboration github repository.

To Clone (Download) the newest version of GRSISort, navigate to the directory where you want the GRSISort directory to be. Clone the repo using:
``````````
git clone https://github.com/GRIFFINCollaboration/GRSISort.git
``````````

GRSISort should now download to **./GRSISort**

Enter into the **GRSISort** directory with `cd GRSISort`.

To see a list of GRSISort versions available for download, use `git tag -l`. The current release will be tagged under `current`.

To download a the version named \<version\>:
```
git checkout <version>
```

*Don't like git? (You should) The newest version can also be downloaded from [[here|https://github.com/pcbend/GRSISort/tarball/master]]*.

<a name="step4" />
## Step 4: Setup Environment Variables
The setup for GRSISort depends on the shell being used. To determine the Shell you are using execute
`````
echo $SHELL
`````
To setup GRSISort for a .sh shell (bash) execute `source thisgrsi.sh` in your terminal from the GRSISort Directory

To avoid running this script every session, add the following to your **~/.bashrc**
```
source /Path/To/GRSISort/thisgrsi.sh
```
To setup GRSISort for a .crsh or .tcsh shell execute `source thisgrsi.csh` in your terminal from the GRSISort Directory

To avoid running this script every session, add the following to your **~/.bashrc**
```
source /Path/To/GRSISort/thisgrsi.csh
```

<a name="step5" />
## Step 5: Compile GRSISort
To Compile GRSISort type `make`. Please be patient as compilation will take a few moments.

- To create the HTML documentation, you can compile with `make docs`. This is NOT REQUIRED.

If you recieve an error, please consult the [Troubleshooting](troubleshooting) page.

<a name="step6" />
## Step 6: ???
Instructions unclear, lost in terminology.

<a name"step7" />
## Step 7: Profit
If you ever figure this out, please share the wealth.