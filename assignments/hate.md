---
layout: page
title: Mapping Hate
permalink: /a/hate/
menu: false
---

In this assignment you will explore the prevalence of hate groups in the US,
investigating possible correlating factors for their prevalence from US Census 
demographic data. The data on hate groups was provided by the [Southern Poverty Law 
Center (SPLC)](http://www.splcenter.org/), a non-profit civil rights organization in Montgomery, Alabama. The remainder of the data was obtained via the [US Census TIGER data
system](https://www.census.gov/geo/maps-data/data/tiger.html). All data is for 2010/2011 and is in CSV (comma separated) or TSV (tab separated)
format with minimal post-processing after retrieval. The functional geospatial unit here 
is a zipcode (ZCTA - "zip code tabulation area" in US Census parlance). We'll also use [Metropolitan Statistical Areas (MSA)](https://en.wikipedia.org/wiki/Metropolitan_statistical_area) for comparison.

For this assignment you will likely turn in either an Rmd ([R Markdown](http://rmarkdown.rstudio.com/)) file and compiled PDF or an iPython notebook. If you choose to use a different environment (e.g., Julia), make sure that you commit everything needed to run and understand what you've done.

## Data

Download the data: [hate.zip](https://drive.google.com/file/d/0B3Vxw_F2RArqN0Jhd0NqZEtBNkE/view?usp=sharing) (52 MB)

 * splc2010.csv - SPLC Data
 * 2010 Census data & metadata
   * census2010.csv - Census Data
   * census2010meta.csv - Column Descriptions
   * census2010info.txt - Additional info
   * census2010*_msa.csv - Same data as above for MSAs
 * 2011 American Community Survey (ACS) data and meta data
   * econ2011.csv - Economic Data
   * econ2011meta.csv - Column Descriptions
   * econ2011info.txt - Additional Info
   * edu2011.csv - Education Data
   * edu2011meta.csv - Column Descriptions
   * edu2011info.txt - Additional Info
 * zipinfo.tsv - Census "Gazette" with additional geo-data on ZCTAs
 * zcta_county.csv - Mapping between ZTCAs and US Counties
 * zcta_msa.csv - Mapping between ZCTAs and MSAs

## References

 * [R Language](http://www.r-project.org/) and [R Package System (CRAN)](http://cran.rstudio.com/)
 * [GGPlot2](http://ggplot2.org/) and [GGMap](http://cran.r-project.org/web/packages/ggmap/ggmap.pdf)
 * [SPLC Hate Map](http://www.splcenter.org/hate-map)
 * [NYT 2010 Census Explorer](http://projects.nytimes.com/census/2010/explorer)
 * [Stop and Frisk: Spatial Analyis of Racial Discrepancies](http://www.r-bloggers.com/stop-and-frisk-spatial-analysis-of-racial-discrepancies/)

## Instructions

### *Part One*

To get you started, we'll walk through some basic data processing and analysis with R. You
don't need to use R to complete this assignment, but it's certainly a good candidate.
Your task is to extend the analysis to include economic and education
variables. Doing so will involve writing routines to load/format/plot/inspect that 
additional data. 

First, let's read in and prepare the SPLC data:

{% highlight r %}
hate <- read.csv('splc2010.csv',header=TRUE)
# simplify the zip column to numeric 5-digit zip
hate$nzip <- sapply(hate$ZIP,function(x){ 
  as.numeric(unlist(strsplit(as.character(x),"-"))[1]) 
})
kkk <- subset(hate,MOVEMENT.CLASS=='Ku Klux Klan')
{% endhighlight %}

Because the KKK is the most prevalent hate group, I've made a separate data frame with just that subset of the data. Next, we can read in the ZCTA-aggregated census data and add a column with counts of hate groups (and KKK):

{% highlight r %}
census <- read.csv('census2010.csv',header=TRUE)

# Count hate groups in various census tracts
census$nhate <- sapply(census$GEO.id2,function(x){ sum(hate$nzip == x) })
census$nkkk <- sapply(census$GEO.id2,function(x){ sum(kkk$nzip == x) })

# Coerce a few variables into being numeric
# Give them better names while we're at it
census$pctwhite <- as.numeric(as.character(census$HD02_S078))
census$pctmale <- as.numeric(as.character(census$HD02_S026))
census$population <- as.numeric(as.character(census$HD01_S001))
{% endhighlight %}  

Let's use the Grammar of Graphics (GGPlot) to take a look at the data and some potential correlations:

{% highlight r %}
library(ggplot2)
    
# Some example plots...

pdf("plots.pdf") # save output to a pdf

# ...Population
qplot(factor(nkkk),population,data=census) + geom_jitter() + 
  xlab("KKK Prevalence") + ylab("Population")

# ...Race
qplot(factor(nkkk),pctwhite,geom="boxplot",data=census) + 
  xlab("KKK Prevalence") + ylab("% White")
qplot(pctwhite,geom="density",color=factor(nhate),data=census) + 
  xlab("% White")

# ...Sex
qplot(factor(nhate),pctmale,geom="boxplot",data=census) 
  + xlab("Hate Group Prevalence") + ylab("% Male")

dev.off() # for pdf

{% endhighlight %}

Take some time to work with the data, try plotting some other variables, or reworking the plots.

**Q1:** Does there appear to be a "sweet" spot for hate group activity vis a vis the size of racial majority?

**Q2:** What is the prevalence of hate groups in zips where the racial spread has a simple majority (>50%) but not a super majority (>66%)?

**Q3:** Does there appear to be a relationship between population and prevalence? If so, what is it?

Next, we can read in some geographic information about the ZCTA regions from the Census Gazette. With lat/lon on hand, we can use GGMap to make some pretty map plots.

{% highlight r %}
# Read in 2010 Census Gazette Info
zip <- read.table("zipinfo.tsv",header=TRUE)

# Determine lat/lon/size for zips
census$lat <- sapply(census$GEO.id2,function(x){ zip[zip$GEOID == x,"INTPTLAT"] })
census$lon <- sapply(census$GEO.id2,function(x){ zip[zip$GEOID == x,"INTPTLONG"] })
census$sqmi <- sapply(census$GEO.id2,function(x){ zip[zip$GEOID == x,"ALAND_SQMI"] })

# Use GGMap to make some maps

library(ggmap)

pdf("maps.pdf")

usa <- qmap("united states",zoom=4)
usa + geom_point(aes(x=lon,y=lat,color=nhate,size=nhate),data=census)

colorado <- qmap("colorado",zoom=7)
colorado + geom_point(aes(x=lon,y=lat,color=nhate,size=nhate),data=census)

dev.off() # for pdf
{% endhighlight %}

**Q4:** Using census$sqmi for zipcode area, what can you say about population density and how it relates to the prevalence of hate groups? Compute some statistics and/or make a plot to demonstrate what you find.

### *Part Two*

Integrate the economic data from econ2011.csv and education data from edu2011.csv.

**Q5:** Are there any interesting correlations between economic factors and hate group prevalence? Compute some statistics and/or make a plot to demonstrate what you find.

**Q6:** Are there any interesting correlations between education attainment and hate group prevalence? Compute some statistics and/or make a plot to demonstrate what you find.

Here's some code to load in census statistics for [Metropolitican Statistical Areas (MSAs)](https://en.wikipedia.org/wiki/Metropolitan_statistical_area):

{% highlight r %}
msa <- read.csv('zcta_msa.csv',header=T)
census.msa <- read.csv('census2010_msa.csv',header=TRUE)
msa <- merge(msa,census.msa,by.x="CBSA",by.y="GEO.id2")

msa$nhate <- sapply(msa$ZCTA5,function(x){ sum(hate$nzip == x) })
msa$pctwhite <- as.numeric(as.character(msa$HD02_S078))
msa$pctmale <- as.numeric(as.character(msa$HD02_S026))
msa$population <- as.numeric(as.character(msa$HD01_S001))
msa$hate.per.person <- msa$nhate/msa$population
msa$hate.per.area <- msa$nhate/msa$MAREALAND
{% endhighlight %}

**Q7:** Try the analyses you did for Q1 - Q3 with the MSA geography. Do you see any differences in the outcome using this (larger) geography? If so, what's different?

## Extra Credit

**EC1:** What methods might you employ to determine which are the most important factors (i.e., economic, education, demographic) for predicting the presence/absence or number of groups in a given zipcode? Using those methods, what can you find?
