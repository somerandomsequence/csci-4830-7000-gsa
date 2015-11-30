---
layout: page
title: Setup Tools
permalink: /a/setup/
menu: false
---

There are a number of GIS and analysis tools we'll use for this course. This assignment is about setting them up. Besides the git repository, I will not verify that you've done these things, but if you don't do them now you'll likely have to do them later.

## Virtual Machine

You have the option of using a virtual machine based on Ubuntu. It can be downloaded here:

[https://foundation.cs.colorado.edu/vm/](https://foundation.cs.colorado.edu/vm/.)

If you're taking other CS courses, you may already have it. Once the VM is loaded and running in [VirtualBox](https://www.virtualbox.org/), this command will install the stash of tools we may use in this course:

```
sudo apt-get update && sudo apt-get install cu-cs-csci-4830-geo -y
```

Alternatively, if you use CSEL machines, this package will already be installed.

If you have problems with the VM environment, you can get help from Andy Saylor (times TBD).

If you feel that using the VM is too slow or onerous and would rather maintain your own copies of the software (e.g., install them directly on your host operating system), you are welcome to do so. However, in that scenario, you will be on your own when it comes to any issues that arise (dependencies, etc.).

## Git

You'll submit your work using the CSCI git server. Create an account here, if you don't have one already:

[https://git.cs.colorado.edu/](https://git.cs.colorado.edu/users/sign_in)

Once you're in, go to this template repository:

[https://git.cs.colorado.edu/phillict/csci-gsa-2016](https://git.cs.colorado.edu/phillict/csci-gsa-2016)

Create a private fork of this repository that is shared with just me and the grader:

  * Click the fork button (to the left of SSH)
  * After forking, go to Settings in the menu on the left
  * Change the Visibility Level to "Private", click "Save" at the bottom
  * Go back to the main project page
  * Go to Members in the menu on the left, click Add Members
  * Add myself (phillict) and the Josh (jofe3744) as Guest members

Once you've created the repository online, check it out to your computer, e.g.:

```
git clone git@git.cs.colorado.edu:youruid/csci-gsa-2016.git
```

Replace youruid in the string above with your CU identikey username.

There are instructions in the README of the template. In sum, you can create a skeleton directory structure by untaring dirs.tar.

When it comes time to do grading on readings and assignments I'll expect to find your submission in the corresponding folder. You'll submit your work by doing a standard:

```
git commit -a
```

To commit all outstanding changes to your local branch. And:

```
git push
```

To submit them upstream to the git server (i.e., push your changes up to the online copy of your repository) so that I can see and grade them. 

## Other Tools

Whether you're using a CSEL machine (where tools should be pre-installed), the Virtual Machine, or your own computer or custom VM, you should check to make sure the necessary pre-requisites are installed. Here's a list:

  * **GIS Stuff**
    * [QGIS](http://www.qgis.org/en/site/) >= 2.6
      * Try: ```qgis```
    * [GRASS](http://grass.osgeo.org/) >= 6.4
      * Try: ```grass```
    * [Proj4](http://trac.osgeo.org/proj/)
      * Try: ```proj```
    * [GDAL](http://www.gdal.org/) >= 1.11
      * Try: ```gdal_translate --help```
    * [Shapelib](http://shapelib.maptools.org/)
      * Try: ```shpdump```
  * **Databases**
    * [PostGIS](http://postgis.net/) >= 2.1
      * Try: ```ls /usr/lib/postgresql/*/lib/postgis*```
    * [PostgreSQL](http://www.postgresql.org/) >= 9.3
      * Try: ```psql -V```
  * **Languages**
    * [Ruby](https://www.ruby-lang.org/en/) >= 2.1
      * Try: ```ruby -v```
    * [R](http://www.r-project.org/) >= 3.1
      * Try: ```R --version``` 
    * [Python](https://www.python.org/) >= 2.7
      * Try: ```python --version```
      * May be easiest to install the [Anaconda distribution](https://www.continuum.io/downloads)
  * **Misc**
    * [Git](http://git-scm.com/) >= 1.9
      * Try: ```git --version```
    * [R Studio Desktop](https://www.rstudio.com/) 
    * [iPython Notebooks](http://ipython.org/)
    
Choosing to install everything yourself, on your own machine or VM is definitely an option, but it is at your own risk. If you run into difficulties (OSX can be a particular headache at times), you should punt to the consistent environment of the standard VM or CSEL machines.
