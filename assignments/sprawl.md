---
layout: page
title: Urban Sprawl
permalink: /a/sprawl/
menu: false
---

In this assignment we will make use of imagery from the [Landsat 7](http://landsat.usgs.gov/about_landsat7.php) satellite to estimate the size of an urban area. Because Landsat 7 has been active since 1999, we can make use of 15 years of successive imagery to plot the growth of [urban sprawl](http://en.wikipedia.org/wiki/Urban_sprawl) in that time frame. We will focus on Dubai, a city in the UAE which has seen [massive growth in those 15 years](http://earthobservatory.nasa.gov/Features/WorldOfChange/dubai.php). We'll use a method of supervised maximum likelihood calculation to determine the extent of human created areas and analyze how those areas grow or change with time.

## Data

Download the data: [sprawl.zip](https://drive.google.com/file/d/0B3Vxw_F2RArqbmU0UUtxN0MtTk0/view?usp=sharing) (418 MB)

  * 1999
    * B1.TIF
    * B2.TIF
    * B3.TIF
    * B4.TIF
    * B5.TIF
    * B6.TIF - actually band 6 w/vcid 2
  * ...
  
## References

  * [Urbanization of Dubai](http://earthobservatory.nasa.gov/Features/WorldOfChange/dubai.php)
  * [Landsat ETM+ Band Definitions](http://landsat.usgs.gov/band_designations_landsat_satellites.php)
  * [Classification with GRASS and QGIS](http://blog.perrygeo.net/2008/01/26/impervious-surface-deliniation-with-grass/)

## Instructions

The provided data was downloaded from [USGS Earth Explorer](http://earthexplorer.usgs.gov/). It's important to note that after May 2003, the Landsat 7 satellite had a [malfunction which caused a reduction in quality](http://landsat.usgs.gov/products_slcoffbackground.php) of images produced after that date. For the purpose of this assignment, we'll proceed as if the images aren't damaged and accept the error in our calculation.