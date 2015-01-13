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

If you have problems with the VM environment, you can get help from Andy Saylor:

  * Monday, 01/12/15 @ 2 PM in ECCS 112 (CSEL South)
  * Tuesday, 01/13/15 @ 11 AM in ECCS 112 (CSEL South)
  * Wednesday, 01/14/15 @ 2 PM in ECCS 112 (CSEL South)
  * Thursday, 01/15/15 @ 11 AM in ECCS 112 (CSEL South)
  * Friday, 01/16/15 @ 4 PM in ECCS 112 (CSEL South)

## Git

You'll submit your work using the CSCI git server. Create an account here, if you don't have one already:

[https://git.cs.colorado.edu/](https://git.cs.colorado.edu/users/sign_in)

Once you're in, create a new **private** project called **csci-4830-7000** (or similar, e.g., csci-4830-7000-geo, if you already have a project called that). 

Next, share it with me (Caleb Phillips) by going to Settings > Members > New Project Member. Guest (read-only) membership will be plenty for me.

Once you've created the repository online, check it out to your computer using the instructions on the main project page. Within the project folder, you'll have a set of directories like so:

  * assignment1
  * assignment2
  * ...
  * reading1
  * reading2
  * ...
  * project
  
When it comes time to do grading on readings and assignments I'll expect to find your submission in the corresponding folder. You'll submit your work by doing a standard:

```
git commit -a
```

To commit all outstanding changes to your local branch. And:

```
git push
```

To submit them upstream to the git server so that I can see and grade them. 

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
  * **Misc**
    * [Git](http://git-scm.com/) >= 1.9
      * Try: ```git --version```
      
Choosing to install everything yourself, on your own machine or VM is definitely an option, but it is at your own risk. If you run into difficulties (OSX can be a particular headache at times), you should punt to the consistent environment of the standard VM or CSEL machines.
