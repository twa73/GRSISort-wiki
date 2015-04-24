#Using GitHub

The way you use git and GitHub will depend on how you plan on interacting with the GRSISort code. If you plan on simply using the code, you should look at the User section. If you plan on developing the code (fixing issues or adding functionality), or just modifying the code for your own use, you should look at the Developer section.

##User

As a user, you will use git and GitHub to make sure that your local version (on your personal computer) is up-to-date with the online repository (on GitHub). The instructions for setting up your local version are well documented on the [[Setting Up GRSISort|Setting-up-GRSISort]] page.

You'll probably want to stay abreast of all of the most recent GRSISort developments though. You can email notifications of updates by registering with GitHub and [“watching” the repository](https://help.github.com/articles/watching-repositories/), or you can just check from the command line. To check if there are any new updates to GRSISort from the command line, go to your GRSISort directory and type:
```
git status
```
You will get a long list of untracked files. These are all the libraries and binaries that you compiled when you typed make. Don't worry about these. DO go up to the top and look at the first few lines of the output. If it has output like this:
```
OUTPUT THAT SAYS YOU ARE behind by so many commits...
```
then you can update your version of GRSISort by typing:
```
git pull.
```
You should get output like this:
```
Updating a59d87f..682462a
Fast-forward
 include/GCanvas.h                                  |   55 ++-
 include/GHistBrowser.h                             |   25 +
 include/GRootCanvas.h                              |    8 +-
 include/GRootCanvasManager.h                       |   47 ++
 include/TCal.h                                     |    2 +-
 include/TChannel.h                                 |    2 +-
 include/TEnergyCal.h                               |    3 +
 include/TFragment.h                                |    2 +-
 include/TGRSITransition.h                          |    2 +
 include/TGRSIint.h                                 |    2 +
 include/TGainMatch.h                               |   38 +-
 include/TNucleus.h                                 |   10 +
 libraries/GROOT/GCanvas.cxx                        |  542 +++++++++++++++++---
 libraries/GROOT/GHistBrowser.cxx                   |   15 +
 libraries/GROOT/GROOTGuiFactory.cxx                |   16 +-
 libraries/GROOT/GROOTLinkDef.h                     |    1 +
 libraries/GROOT/GRootCanvas.cxx                    |   78 ++-
 libraries/GROOT/GRootCanvasManager.cxx             |   60 +++
 libraries/GROOT/makefile                           |    2 +-
 libraries/TGRSIAnalysis/TCal/TEnergyCal.cxx        |   19 +
 libraries/TGRSIAnalysis/TCal/TGainMatch.cxx        |  434 ++++++++++++++--
 .../TGRSIAnalysis/TNucleus/TGRSITransition.cxx     |   11 +
 libraries/TGRSIAnalysis/TNucleus/TNucleus.cxx      |   76 +++-
 libraries/TGRSIFormat/TChannel.cxx                 |   28 +-
 libraries/TGRSIFormat/TFragment.cxx                |   12 +-
 libraries/TGRSIint/TGRSIint.cxx                    |   20 +
 util/MakeMatrices.C                                |   41 +-
 util/MakeTimeDiffSpec.C                            |   11 +-
 util/offsetfix.C                                   |    4 +-
 29 files changed, 1343 insertions(+), 223 deletions(-)
 create mode 100644 include/GHistBrowser.h
 create mode 100644 include/GRootCanvasManager.h
 create mode 100644 libraries/GROOT/GHistBrowser.cxx
 create mode 100644 libraries/GROOT/GRootCanvasManager.cxx
```

##Developer

As a developer, you will use git and GitHub to make sure your local version is up-to-date with the online repository, but you will also be modifying the code and potentially submitting changes to the online repository. To do this, you will need a more detailed understanding of git and GitHub. There are some resources online (see [here](http://rogerdudler.github.io/git-guide/) and [here](https://www.codeschool.com/courses/try-git), for example) that will be useful.

###Forking the common repository
The best path forward is to  to create your own account on GitHub and [fork the repository] (https://help.github.com/articles/fork-a-repo/). This creates your own online repository that you can play with while not messing up the common repository. It also allows you to submit changes to the common repository. To do this, register with GitHub and then navigate to the GRSISort webpage. In the upper right-hand corner, click the “Fork” button. You should see your own version of this repository now.

###Downloading a copy of your personal repository
To install a local copy of your personal repository, follow the instructions in the Setting Up GRSISort page except instead of cloning `https://github.com/pcbend/GRSISort.git`, clone `https://github.com/YOUR-USER-NAME/GRSISort.git`.

###Updating your code from the common repository
To this, you must first tell git where the upstream (common) repository is. This is succinctly and clearly explained in GitHub's [Configuring a remote for a fork](https://help.github.com/articles/configuring-a-remote-for-a-fork/) help page.

The next step is to download the changes from the common repository. This is called [syncing a fork](https://help.github.com/articles/syncing-a-fork/). *(Advanced: check out the local branch you want to update first.)* To do this, use the following command:
```
git pull upstream master
```
This downloads (fetches) a copy of the changes (commits) from the common (upstream) repository. It also merges those commits into your own local copy of the code. If there are commits that conflict with changes you have made to your local repository, you will be asked to [resolve those conflicts](https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line/).

The last step is to update your online repository. In executing the pull, you updated your local copy, but your personal repository still doesn't have these changes committed. To commit these updates to your local copy, use:
```
git push origin master
```
This pushes the changes from your local master branch to the online repository identified as origin. To see a list of the repositories you are connected to, use `git remote -v`.

###Making changes to your personal online repository
Add any files that you have created or modified to your staging area with:
```
git add FILENAME1 FILENAME2 …
```
Then update your online repository with the modifications specified in the staging area:
```
git commit -m “Quick message that summarizes these changes.”
```
If you do not use the `-m` flag, git will open up a text editor and ask you to write a commit message there. The point of this is to record exactly what changed and when – enforced documentation!

###Suggesting a change to the common repository
To suggest a change to the common repository, first make sure your local copy has the most recent changes from the common repository. Then, update your personal online repository with the suggested changes. Finally, [create a pull request](https://help.github.com/articles/creating-a-pull-request/). An administrator will look over the code and merge it into the common online repository or discuss possible modifications to you proposed changes.