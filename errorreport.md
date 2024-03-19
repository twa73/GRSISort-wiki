[**HOME**](Home) > [**TROUBLESHOOTING**](TroubleShooting) > [**ERROR REPORT**](errorreport)

**Error Reporting**

To report and error on the issue tracker, please use the error reporting template provided. To Automatically fill this out, use `sh ${GRSISYS}/util/ErrorReport.sh`. This will generate a template that has many parts automatically filled out, and looks like:
```
SYSTEM = Darwin
GRSISYS = /Users/rdunlop/Programs/GRSISort
ROOTSYS = /usr/local/root_v5.34.30
ROOT Version = 5.34/30

Computer and Path to File that failed:

GRSISort Branch = main

Last Commit:
commit 479204ae1385a874abda8f32bc41eef354e0f20e
Author: Ryan Dunlop <rdunlop@uoguelph.ca>
Date:   Wed Oct 21 11:04:20 2015 -0400

    Fixed error reporting script

Put Error Here:




Last Working Commit: Put commit here


What I have tried so far


```
Please add: 
- The computer and path you used (if applicable)
- The output error you received
- The last working commit (this really really helps us)
- Anything you have tried to solve the problem and results from those tries

Please use this as your first post when reporting an error on the issue tracker as it will help us save time in solving your problem.
