---
layout: page
title: Urban Sprawl
permalink: /a/sprawl/
menu: false
---

In this assignment we will make use of imagery from the [Landsat 7](http://landsat.usgs.gov/about_landsat7.php) satellite to estimate the size of an urban area. Because Landsat 7 has been active since 1999, we can make use of 15 years of successive imagery to plot the growth of [urban sprawl](http://en.wikipedia.org/wiki/Urban_sprawl) in that time frame. We will focus on Dubai, a city in the UAE which has seen [massive growth in those 15 years](http://earthobservatory.nasa.gov/Features/WorldOfChange/dubai.php). We'll use a method of supervised maximum likelihood calculation to determine the extent of human created areas and analyze how those areas grow or change with time.

## Data

Download the data: [sprawl.zip](https://drive.google.com/file/d/0B3Vxw_F2RArqbmU0UUtxN0MtTk0/view?usp=sharing) (418 MB)

  * 1999-09-22
    * B1.TIF
    * B2.TIF
    * B3.TIF
    * B4.TIF
    * B5.TIF
    * B6.TIF - actually band 6 w/vcid 2
    * B7.TIF
    * B8.TIF
  * ...
  
Each directory contains an 8-band Landsat 7 image separated into 1-band rasters. Bands 1, 2, and 3 are the visible spectrum (green, blue, and red respectively). The provided data was downloaded from [USGS Earth Explorer](http://earthexplorer.usgs.gov/). It's important to note that after May 2003, the Landsat 7 satellite had a [malfunction which caused a reduction in quality](http://landsat.usgs.gov/products_slcoffbackground.php) of images produced after that date. For the purpose of this assignment, we'll proceed as if the images aren't damaged and accept the error in our calculation.

If you wanted to, say, generate an full color image of the 15 years of development, you could use a script like this:

{% highlight bash %}
#!/bin/bash
for dir in *;do
  if [ ! -d $dir ];then
    continue
  fi
  echo $dir
  gdalbuildvrt -separate $dir/RGB.VRT $dir/B3.TIF $dir/B2.TIF $dir/B1.TIF
  gdal_translate -of PNG $dir/RGB.VRT $dir.png
done
convert -set delay 75 -dispose 1 -loop 0 *.png film.gif
{% endhighlight %}
  
## References

  * [Urbanization of Dubai](http://earthobservatory.nasa.gov/Features/WorldOfChange/dubai.php)
  * [Landsat ETM+ Band Definitions](http://landsat.usgs.gov/band_designations_landsat_satellites.php)
  * [Classification with GRASS and QGIS](http://blog.perrygeo.net/2008/01/26/impervious-surface-deliniation-with-grass/)

## Instructions

Your task is to use the maximum likelihood classifier in GRASS to do partially supervised classification of the 8-band images so that we can estimate the percentage of (1) impervious surface (human created things) (2) dry, natural land and (3) water at each point in time. I'm going to advocate the combination of GRASS and QGIS with some scripting to get the task done, but you're also welcome to roll your own image classifier, or use something else entirely (so long as you can justify its accuracy).

The first step is to have a look at the data. Load it into QGIS. You will need to create a new (polygon) vector layer (shape file) of training shapes to feed to to maximum likelihood classifier. You can use the default attribute named "id", assigning a 1 for impervious surface, 2 for dry land, and 3 for water.

Next, you'll need to get to work with GRASS to do the classification. As discussed [here](http://blog.perrygeo.net/2008/01/26/impervious-surface-deliniation-with-grass/), an outline of the process for a single multi-band image is:

{% highlight bash %}
# Create a group of images called 'test' containing all 8 rasters
i.group group=test subgroup=test input=test.1@PERMANENT,test.2@PERMANENT,test.3@PERMANENT,test.4@PERMANENT,test.5@PERMANENT,test.6@PERMANENT,test.7@PERMANENT,test.8@PERMANENT
# Align the regions and select the group
i.target -c group=test
g.region vect=classify align=test.1 -p
# Convert the training vector into a raster
v.to.rast in=classify out=classify use=attr col=id --o
# Compute the spectral signature (mean per-band intensities and covariance matrix)
i.gensig group=test subgroup=test sig=classify.sig training=classify
# Perform Maximum-Likelihood classification and save the resulting raster in test.out
i.maxlik group=test subgroup=test sig=classify.sig class=test.out
{% endhighlight %}

This example assumes that you've loaded in each of the 8 bands to separate rasters named test.1, test.2 and so on, and that you've loaded in your training shapefile into something called classify. If you wish, you may want to perform some small smoothing using neighbor-based averaging:

```
r.neighbors input=test.out output=test.out method=mode size=3
```

Take a look at classify.sig (you can find it in your GRASS directory---try a "find . -name *.sig). The format of this file is provided [here](http://grass.osgeo.org/grass64/manuals/i.gensig.html). 

**Q1**: Can you provide a plot of mean value for each band for each class? Which bands seem to be most critical?

Once you have a working example for one image, you can use a GRASS script to automate the process of calculating the classified raster for each image. When you do this, you'll need to either create a separate training shapefile for each image (ugh), or create a single shapefile that works for all images. Here's an example GRASS script as a starting point:

{% highlight bash %}
#!/bin/bash

# path to GRASS binaries and libraries:
export GISBASE=/Applications/GRASS-6.4.app/Contents/MacOS

export PATH=$PATH:$GISBASE/bin:$GISBASE/scripts
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$GISBASE/lib

# use process ID (PID) as lock file number:
export GIS_LOCK=$

# settings for graphical output to PNG file (optional)
export GRASS_PNGFILE=/tmp/grass6output.png
export GRASS_TRUECOLOR=TRUE
export GRASS_WIDTH=900
export GRASS_HEIGHT=1200
export GRASS_PNG_COMPRESSION=1
export GRASS_MESSAGE_FORMAT=plain

# path to GRASS settings file
export GISRC=$HOME/.grassrc6

g.version
{% endhighlight %}

Make sure to fix GISBASE to match where grass lives for you.

**Q2**: How effective is the classification? Based on your experimentation, what seems to impact its accuracy? How would the approach differ in another area, or with different images?

Next, use the classified rasters (perhaps converted to vector files first) to compute the percentage of each area. 

**Q3**: What is the rate of growth of Dubai in this time period? Provide a plot of percentage impervious versus time.

## Extra Credit

**EC1**: Perform a similar analysis for a different city (e.g., Las Vegas or Phoenix), or with different satellite imagery and compare the methods and results.

**EC2**: Download Phoenix orthoimagery (for just one year, with minimal cloud cover). Use the same method to train a classifier for swimming pools. How many swimming pools are in Pheonix? What is their combined surface area? Assuming an average depth of 5 feet, what is their total volume? (By way of comparison, Lake Mead currently stores 9,661,950 acre-ft of water. Empirical evaporation rates are [given here](http://www.wrcc.dri.edu/htmlfiles/westevap.final.html))
