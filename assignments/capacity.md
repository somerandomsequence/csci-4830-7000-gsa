---
layout: page
title: Carrying Capacity
permalink: /a/capacity/
menu: false
---

This assignment explores the concept of local/regional carrying capacity, i.e., how
many animals (people, in this case) can be supported by the land containing a given population center. We'll focus on Hawaii since it is an isolated state where any food not 
grown locally must be delivered by freighter. Land-cover classification (NLCD) data from 2001 will be the basis of our analysis.

## Data

Download the data: [capacity.zip](https://drive.google.com/file/d/0B3Vxw_F2RArqTmt6dGRaR1Frb3c/view?usp=sharing) (2 MB)

  * hi_landcover_wimperv_9-30-08_se5.img - Raster landcover data
  * landcov_meta_web_final - Directory containing meta-data
 
## References

  * [Shapely](https://pypi.python.org/pypi/Shapely) and [Fiona](ttp://www.macwright.org/2012/10/31/gis-with-python-shapely-fiona.html)

## Instructions

Start by reading the following paper:

> C. Peters, J. Wilkens, G. Fick. Testing a complete-diet model for estimating the land 
> resource requirements of food consumption and agricultural carrying capacity: The New 
> York State example. Renewable Agriculture and Food Systems: 22(2); 145-153. 2007. 
> Download: [PDF](/pdfs/Peters2007.pdf).

We'll use Land-Cover data to perform a simplified version of this study, focused on Hawaii. The main simplification is that you'll choose a single nutritional model to analyze based on your own discretion. You will use land requirement estimates from Table 2 in the Peters paper.

Your first task is to convert the given NLCD raster to a vectorized format. You can do that with QGIS (Raster -> Transform -> ...) or with GDAL (gdal_polygonize.py). However you do it, load the files into QGIS and have a look at them.

**Q1**: What is the spatial projection used in the provided raster? What are the units of distance for this projection?

Next, you need to look at the provided meta-data to understand what each landcover class means. For instance, class 41 (i.e., polygons in your raster with class 41) are areas dominated by trees greater than 20 meters tall.

**Q2**: Which classes are arable land? 

**Q3**: Which (arable) classes are better for ruminants (grazing livestock)?

**Q4**: Which classes might offer an opportunity for small-scale urban agriculture? What percentage of these class's area might reasonably be used for growing food?

**Q5**: Make a plan about how each of these classes could be ideally utilized for a complete local agriculture system focused on self-sufficiency. Where will vegetables, oils, fruits, and pulses/legumes be grown? Will there be livestock? Justify your plan.

Let's write a script to calculate the carrying by reading in the shapefile using the shapely. First you'll want to make sure you have shapely and fiona installed. You can use pip to do that and if you don't have pip you can [install it using these instructions](https://pip.pypa.io/en/latest/installing.html).

{% highlight bash %}
sudo pip install shapely
sudo pip install fiona
{% endhighlight %}

On a machine you don't have root access to, you can use the "--user" option to pip and drop the sudo.

Here's the start of a script that uses julia and shapely to read in a shapefile and loop over the shapes:

{% highlight Python %}
#!/usr/bin/env python

from shapely.geometry import mapping, shape
from fiona import collection

with collection("land_cover.shp","r") as input:
  for polygon in input:
    c = polygon['properties']['class']
    if c == 0 or c == None:
      continue
    s = shape(polygon['geometry'])
    print s
{% endhighlight %}

Extend this script to calculate the combined area for each arable class you identified. Then using Mcal/m^2 numbers from Table 2 of the Peters paper, and your assumed food system plan calculate total productivity.

**Q6** Assuming a Mcal/acre model for productivity for each of these categories based on your idealized food system, what is the total potential productivity in calories for Hawaii? 

**Q7** How much does this fall short of the presumed caloric requirements of this region (use population data for the same year as the LCDB data). What is the carrying capacity?

**EC1:** Download the NLCD 2011 data for the contiguous United States [here](http://www.mrlc.gov/nlcd11_data.php). Use this data to make a carrying capacity estimate for the entire United States. Optionally, extend your analysis to compare the 50 contiguous states in their ability to self-support their populations. Discuss your findings.

**EC2:** Load your shapefile into [PostGIS](http://postgis.net/) using the [shp2pgsql tool](http://suite.opengeo.org/4.1/dataadmin/pgGettingStarted/shp2pgsql.html). Provide the SQL commands necessary to compute the carrying capacity matching your earlier results with PostGIS.
