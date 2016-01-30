---
layout: page
title: Personal Census
permalink: /a/census/
menu: false
---

This assignment involves using the US Census data explorer to obtain a small amount of personally relevant information and to summarize it. You'll provide summaries of a few basic demographics for the place where you live at multiple scales to understand whether the demographic of where you live on a small scale is representative of larger (containing) regions. The point of the assignment is to orient you to how public data is made available online (by e.g., the Census Bureau), how it is formatted, and how it can be used.

## Data

Go to the US Census Bureau American Fact Finder to download data from the US Census. I generally find that the Advanced Search function is easier to use:

[US Census Advanced Data Search](http://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml?refresh=t)

Use the Geography selector to put in your current address. If you live on campus, use an address for a place you lived previously. If you live on campus now and previously lived in a different country, you can either use equivalent data for that country (if available), or pick an address arbitrarily in a city you're intrested in.

Using the tool, you should see that data is available at a number of resolutions. Add these geographies:

  * State
  * County
  * Metro Area (or City)
  * ZTCA (Zip Code Tabulation Area)
  * Census Tract
  * Census Block Group

Next, use the Topics selector to limit to data from Year 2013.

In the main pane select:

  * Median Household Income
  * Sex by Age

Click Download near the bottom of the page. This should start a process which has you download a zipfile containing 6 files. There are 2 spreadsheets with the actual data, and 4 files with column descriptions and meta data.

## References

  * [US Boundry](http://www.usboundary.com/) - Census data visualization, nice for visualizing tract-level data
  * [2010 Census](https://projects.nytimes.com/census/2010/map) - NYT visualization, with block-level data
  * [Social Explorer](http://www.socialexplorer.com/) - More general version of the above

## Analysis

You can do the analysis using any tool of your choosing, but a spreadsheet will probably be the easiest. Save the data in your git repository under the right assignment directory and put the answers to the questions below in a [Markdown document](https://help.github.com/articles/markdown-basics/) named analysis.md.

**Q1:** What is the percentage of 18-24 year old people (irrespective of gender/sex) in each geography?

**Q2:** Are any of the geographies meaningfully different from the rest, or are they all fairly similar? If they are different, why do you think that geography is different in terms of the fraction of college-aged people?

**Q3:** What is the median household income for each geography? Again, any outliers? If so, what do you think is skewing the income up or down in those geographies?

**Q4:** Considering both age and income, is your block group or census tract more representative in terms of the city average? What about the state average?

## Extra Credit

**EC1:** Do an analysis similar to the above for household tenure (number of people living in each house), race, or any other demographic of interest.

**EC2:** Make a [dotplot](http://www.statmethods.net/graphs/dot.html) of the proportions for each geography to simply visualize the change with scale.

**EC3:** Utilize error margins provided in the Census data to put error bars (upper and lower estimates) on your summary statistics.
