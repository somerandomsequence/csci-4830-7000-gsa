---
layout: page
title: American Guts
permalink: /a/guts/
menu: false
---

In this project, you will utilize data from the [American Gut](http://americangut.org), with the goal of understanding
whether human gut microbiome is spatially autocorrelated (i.e., whether folks who live near one another have similar
gut flora).

## Data

Data for the American Gut project is open and can be obtained online in this [github repository](https://github.com/biocore/American-Gut/tree/master/data/AG).

You'll want to grab the files AG.txt, which contains the complete metadata, and AG.biom, which contains the OTU table. The OTU table an HDF5 file with
a particular format that is used by microbiologists for storing OTU data and metadata. It can be parsed with a Python library called 'biom', which you'll
need to install (along with pandas, and numpy if you don't already have them):

{% highlight bash %}
pip install biom
pip install pandas
pip install numpy
{% endhighlight %}

In the OTU table, there is one row for each participant (sample). Each column of the table corresponds to a particular microbe/taxanomic identifier. The values
in the matrix are the counts of occurances of each microbe in that sample. The way this sort of sequencing works, a set number of microbes' RNA can be detected
in each sample. In practice, that means that each row will have the same sum, and any given count can be thought of as *proportional to the population of that
microbe in the sample*.

Here's some example python code to process the data and meta data and create one CSV for analysis:

{% highlight python %}
import biom
import csv
import numpy
import pandas

# Read in the OTU data and metadata
ag = biom.load_table('AG.biom')
meta = csv.DictReader(open('AG.txt'), delimiter="\t")

# For each sample in the metadata, extract the fields we care about
locs = {}
for row in meta:
    locs[row["#SampleID"]] = {"lat": row["LATITUDE"],
                              "lon": row["LONGITUDE"],
                              "site": row["BODY_SITE"],
                              "diet": row["DIET_TYPE"]}

# For each sample in the OTU data, count the number of nonzero taxa
for values, i, metadata in ag.iter():
    if i in locs.keys():
      locs[i]["nz"] = numpy.count_nonzero(values)

# Write it to a CSV
df = pandas.DataFrame(locs).transpose()
df.to_csv("prepped.csv")
{% endhighlight %}

Here we're outputing a simple statistic for analysis, the number of nonzero taxa, which can be thought of as the diversity (number of unique taxa observed) in each sample.
 
## References

  * [American Gut Data Analysis](http://americangut.org/?p=189)
  * [Biom Fime Format](http://biom-format.org/documentation/index.html)
  * [Operational Taxonomic Unit (OTU)](https://en.wikipedia.org/wiki/Operational_taxonomic_unit)
  * Quick R Guide to Cluster Analysis](http://www.statmethods.net/advstats/cluster.html)

## Instructions

For this project, you can use the environment of your choice for analysis. R is a good choice because it has easy geovisualizations with ggmap and has libraries for spatial statistics. Python also has a library for spatial statistics called [PySAL](http://pysal.readthedocs.org/en/latest/users/tutorials/autocorrelation.html).

Start by making a simple scatter plot map of the data. Here's an example in R:

{% highlight R %}
library(ggplot2)
library(ggmap)
library(dplyr)

diversity <- read.csv('diversity.csv',header=T)
diversity <- mutate(diversity,lat=as.numeric(as.character(lat)),
                    lon=as.numeric(as.character(lon)))

usa <- get_map("USA",zoom=4)
ggmap(usa) + geom_point(aes(x=lon,y=lat,color=nz),data=diversity)
{% endhighlight %}

You can also make a choropleth map if you prefer.

Q1: How is the data distributed around the USA? Describe spatial biases and the dispersion process.

Next, compute either Moran's I or Geary's C on the data. Make sure you analyze data from different body sites separately (i.e., skin v.s. fecal).

Q2: Is the diversity data spatially autocorrelated? How does the test you've done support this (or not)?

Now, modify the python code above to calculate the difference between each *pair* of samples. The output should be an N x N matrix, where N is the number of samples in the metadata (if you don't limit your output to those samples in the metadata, the computation will take a very long time!). For the operating definition of distance, we'll use the [Tanimoto Distance Coefficient](https://en.wikipedia.org/wiki/Jaccard_index#Tanimoto.27s_definitions_of_similarity_and_distance).

So that it completes in a reasonable amount of time, you'll need to use some care with how you compute the Tanimoto coefficient. Here are two functions that will help---one converts an OTU vector of counts to a bit string stored in a long integer, the other calculates the Tanimoto distance on two such bit strings. Even with this optimization, the computation may take an hour or more to run on your machine.

{% highlight python %}
import math

def otus_to_bits(v):
  return reduce(lambda x,y: x|y,map(lambda (i,x): 1<<i if x > 0 else 0,enumerate(v)))

def tanimoto(v1,v2):
  ts = float(bin(v1 & v2).count("1"))/bin(v1 | v2).count("1")
  return None if ts == 0 else -1*math.log(ts,2)
{% endhighlight %}

The final bit of this assignment is a cluster-analysis. We want to cluster the samples using the distance matrix and see if cluster membership seems to be correlated with metadata. So that you can complete the cluster analysis in a reasonable amount of time, take the data from a subset of 500 samples selected from the same body site (your choice).

Q3: Are there any metadata that you expect will be correlated with how the samples cluster using the Tanimoto distance?

Tip: you can easily see all the metadata fields with:

{% highlight bash %}
head -n 1 AG.txt
{% endhighlight %}

Use R, Python or a method of your choice to cluster the data. If you use R, the CLARA and PAM methods are both good choices and can be found in the [cluster package](https://cran.r-project.org/web/packages/cluster/cluster.pdf). CLARA will be faster than PAM due to the way it clusters random sub-samples. Both methods can automatically determine the optimal number of clusters. For python, [scikit learn](http://scikit-learn.org/stable/modules/clustering.html) has excellent clustering support.

Q4: How many clusters seem ideal for this data? 

Q5: Do you see any clear correlations between the outcome of clustering and metadata (e.g., site, age, sex, diet, etc.)? Imagine you were reporting the initial results of a contracted cluster analysis to a client. What would you tell them?

## Extra Credit

* EC1: Extend the above analysis, making use of the taxonomic metadata (which OTUs are related, and how closely) to define a new diversity metric.

* EC2: Can you predict cluster membership using some combination of metadata fields? Try a machine-learning classification model with a handful of features and report on its accuracy. Don't go off the deep end unless you want to!
