[**HOME**](Home) > [**GRSISORT SETUP GUIDE**](Setting-up-GRSISort)

Setting up GRSISort is a five step process:

1. [Install git](#step1)
2. [Install Root](#step2)
3. [Download GRSISort](#step3)
4. [Setup Environment Variables](#step4)
5. [Compile GRSISort](#step5)

<a name="step1" />
## Step 1: Install git

To Install git, go to [GitHub](http://github.com) and click on *Download*.

Alternatively, Ubuntu users can use
```````
sudo apt-get install git
```````

<a name="step2" />
##Step 2: Install ROOT
To download and install root follow the directions on the [ROOT](http://root.cern.ch/drupal/content/installing-root-source) website.

*Already have ROOT? Proceed to [Download GRSISort](#step3)*

<a name="step3" />
## Step 3: Download GRSISort

The newest version of GRSISort currently resides on P.C. Bender's github repository.

To Clone (Download) the newest version of GRSISort, navigate to the directory where you want the GRSISort directory to be. Clone the repo using:
``````````
git clone https://github.com/pcbend/GRSISort.git
``````````

GRSISort should now download to **./GRSISort**


<a name="step4" />
## Step 4: Setup Environment Variables
The setup for GRSISort depends on the shell being used. To determine the Shell you are using execute
`````
echo $SHELL
`````
To setup GRSISort for a .sh shell (bash) execute `source SOURCEME.sh` in your terminal from the GRSISort Directory

To avoid running this script every session, add the following to your **~/.bashrc**
```
export GRSISYS=/path/to/dir/of/GRSISort
export PATH=$GRSISYS/bin:$PATH
export LD_LIBRARY_PATH=$GRSISYS/libraries:$LD_LIBRARY_PATH
```

To compile using a .crsh or .tcsh shell execute `source SOURCEME.csh` in your terminal from the GRSISort Directory

To avoid running this script every session, add the following in your **~/.cshrc**  or **~/.tcshrc** 
```
setenv GRSISYS /path/to/dir/of/GRSISort
set path = ($path $GRSISYS/bin)
setenv LD_LIBRARY_PATH ${LD_LIBRARY_PATH}:$GRSISYS/libraries
```

<a name="step5" />
## Step 5: Compile GRSISort
To Compile GRSISort type `make`. Please be patient as compilation will take a few moments.
If you recieve an error, please consult the [Troubleshooting](troubleshooting) page.

