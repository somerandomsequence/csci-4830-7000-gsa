---
layout: page
title: Personal Census
permalink: /a/census/
menu: false
---

This assignment involves using the US Census data explorer to obtain a small amount of personally relevant information and to summarize it. You'll provide summaries of a few basic demographics of the place where you live at multiple scales to understand whether the demographic of where you live on a small scale is representative of larger regions. The point of the assignment is to orient you to how public data is made available online, and in what formats.

## Data

Go to the US Census Bureau American Fact Finder to download data from the US Census. I generally find that the Advanced Search function is easier to use:

[US Census Advanced Data Search](http://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml?refresh=t)

Use the Geography selector to put in your current address. If you live on campus, us the address where you grew up instead.

You should see that data is available at a number of resolutions. Add:

  * State
  * County
  * Metro Area (or City)
  * ZTCA (Zip Code Tabulation Area)
  * Census Tract
  * Census Block Group

Use the Topics selector to limit to data from Year 2013.

In the main pane select:

  * Median Household Income
  * Sex by Age

Click Download near the bottom of the page. This should start a process which has you download a zipfile containing 6 files. There are 2 spreadsheets with the actual data, and 4 files with column descriptions and meta data.

## Analysis

You can do the analysis using any tool of your choosing, but a spreadsheet will probably be the easiest. Save the data in your git repository under the right assignment directory and put the answers to the questions below in a [Markdown document](https://help.github.com/articles/markdown-basics/) named analysis.md.

**Q1:** What is the percentage of 18-24 year old people (irrespective of gender/sex) in each geography?

**Q2:** Are any of the geographies meaningfully different from the rest, or are they all fairly similar? If they are different, why do you think that geography is different in terms of the fraction of college-aged people?

**Q3:** What is the median household income for each geography? Again, any outliers? If so, what do you think is skewing the income up or down in those geographies?

**Q4:** Considering both age and income, is your block group or census tract more representative in terms of the city average? What about the state average?
