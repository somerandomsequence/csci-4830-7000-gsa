---
layout: page
title: Final Project
permalink: /a/project/
menu: false
---

The second half of the course will give you the opportunity to seek out a project
of your choosing and investigate it with some thoroughness.

The guidelines for the project selection are fairly open: it must involve some geospatial
data analysis, and it must be of a reasonable size and shape for the class.

**Undergraduate students** are expected to select a problem demonstrating some ambition and requiring a reasonable amount of problem solving and engineering. Undergraduate students should be aware of some relevant, related research in the area.

**Graduate students** are expected to do work of some novelty and importance, ideally with potential for publishing. Those students with their own research (and theses) are encouraged to select something that overlaps with their existing area of expertise if thats desirable. Graduate students are expected to survey the literature to a sufficient extent that they can describe how the question they've investigated advances the state of the art, or complements existing methods.

In both cases students are welcome to extend current projects from their own research, open sourcery, or other classes with geospatial analysis (so long as the geospatial addition constitutes meaningful work and makes sense for the project).

There are some important events and deliverables:

  * **Proposal** - You'll tell the class what you want to do and solicit feedback
  * **Abstract** - 1 page abstract (no longer) describing project aims will be due at the start. This should be in LaTeX format with references as necessary.
  * **Status** - You'll check in with the class and give a short, detailed update on your project several times. Come prepared with visuals and demos. At the later statuses, you'll also send me rough drafts of your report.
  * **Talk/Paper** - Final results will be presented as a final paper in the format of a technical conference paper of approximately 8-12 pages, single spaced, two-column with figures and references included.
  
## Inspiration

Not sure what you want to do? As you think about a project you may find [this giant list of free GIS data](http://freegisdata.rtwilson.com/) helpful. The newspaper can provide some inspiration vis a vis interesting current social analysis, and the website [FiveThirtyEight](http://fivethirtyeight.com) sometimes has inspiring data examples (although you have to avoid all the sportsball references) and the [Nasa Earth Observatory](http://earthobservatory.nasa.gov/Features/?eocn=topnav&eoci=features) has tons of fantastic remote sensing examples. As a last resort I'm happy to offer some project ideas, although it's most helpful if you come to the discussion with a couple directions in mind.

## Expectations

Status reports are intended to keep your pace of work and progress even throughout the 6-8 weeks of project development. You're encouraged to make progress on your project each week and budget your time to allow that. 

If you feel you are having problems with your project or may not be able to complete the goals you set out for, it is your responsibility to set up a time to communicate those concerns to me. Since the project is a large chunk of the grade, it's preferable to consider approaches to pivoting or remediation early on.

## Template

You'll use [LaTeX](http://www.latex-project.org/) to typeset and format your document. The following archive contains an template (example.tex) and starting Makefile:

  * [project.zip](/a/project.zip)

To compile the LaTeX file into a PDF, you can simply:

```
make build
```

Here's an abbreviated version of the template to be used for the abstract:

{% highlight latex %}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% Journal Article
% LaTeX Template
% Version 1.3 (9/9/13)
%
% This template has been downloaded from:
% http://www.LaTeXTemplates.com
%
% Original author:
% Frits Wenneker (http://www.howtotex.com)
% Modified by:
% Caleb Phillips (caleb.phillips@colorado.edu)
%
% License:
% CC BY-NC-SA 3.0 (http://creativecommons.org/licenses/by-nc-sa/3.0/)
%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

%----------------------------------------------------------------------------------------
%	PACKAGES AND OTHER DOCUMENT CONFIGURATIONS
%----------------------------------------------------------------------------------------

\documentclass[twoside]{article}

\usepackage{lipsum} % Package to generate dummy text throughout this template

\usepackage[sc]{mathpazo} % Use the Palatino font
\usepackage[T1]{fontenc} % Use 8-bit encoding that has 256 glyphs
\linespread{1.05} % Line spacing - Palatino needs more space between lines
\usepackage{microtype} % Slightly tweak font spacing for aesthetics

\usepackage[hmarginratio=1:1,top=32mm,columnsep=20pt]{geometry} % Document margins
\usepackage{multicol} % Used for the two-column layout of the document
\usepackage[hang, small,labelfont=bf,up,textfont=it,up]{caption} % Custom captions under/above floats in tables or figures
\usepackage{booktabs} % Horizontal rules in tables
\usepackage{float} % Required for tables and figures in the multi-column environment - they need to be placed in specific locations with the [H] (e.g. \begin{table}[H])
\usepackage{hyperref} % For hyperlinks in the PDF

\usepackage{lettrine} % The lettrine is the first enlarged letter at the beginning of the text
\usepackage{paralist} % Used for the compactitem environment which makes bullet points with less space between them
\usepackage{graphicx} % Used for including images

\usepackage{abstract} % Allows abstract customization
\renewcommand{\abstractnamefont}{\normalfont\bfseries} % Set the "Abstract" text to bold
\renewcommand{\abstracttextfont}{\normalfont\small\itshape} % Set the abstract itself to small italic text

\usepackage{titlesec} % Allows customization of titles
\renewcommand\thesection{\Roman{section}} % Roman numerals for the sections
\renewcommand\thesubsection{\Roman{subsection}} % Roman numerals for subsections
\titleformat{\section}[block]{\large\scshape\centering}{\thesection.}{1em}{} % Change the look of the section titles
\titleformat{\subsection}[block]{\large}{\thesubsection.}{1em}{} % Change the look of the section titles

\usepackage{fancyhdr} % Headers and footers
\pagestyle{fancy} % All pages have headers and footers
\fancyhead{} % Blank out the default header
\fancyfoot{} % Blank out the default footer
\fancyfoot[RO,LE]{\thepage} % Custom footer text

%----------------------------------------------------------------------------------------
%	TITLE SECTION
%----------------------------------------------------------------------------------------

\fancyhead[C]{CSCI 4830/7000 $\bullet$ Spring 2015 $\bullet$ Jane Doe} % FIXME: Custom header text
\title{\vspace{-15mm}\fontsize{24pt}{10pt}\selectfont\textbf{My Sweet Project}} % FIXME: Article title
\author{
\large
\textsc{Jane Smith} \\ % Your name
\normalsize University of Colorado, Boulder \\ % Your institution
\normalsize \href{mailto:john@smith.com}{john@smith.com} % Your email address
\vspace{-5mm}
}
\date{}

%----------------------------------------------------------------------------------------

\begin{document}

\maketitle % Insert title

\thispagestyle{fancy} % All pages have headers and footers

%----------------------------------------------------------------------------------------
%	ABSTRACT
%----------------------------------------------------------------------------------------

\begin{abstract}

\noindent \lipsum[1] % Dummy abstract text
\lipsum[2]
\lipsum[3]
\lipsum[4]
\lipsum[5]
You can refer to citations like this: \cite{figueredo2009}.

\end{abstract}

%----------------------------------------------------------------------------------------
%	REFERENCE LIST
%----------------------------------------------------------------------------------------

\begin{thebibliography}{99} % Bibliography - this is intentionally simple in this template

\bibitem[1]{figueredo2009}
Figueredo, A.~J. and Wolf, P. S.~A. (2009). Assortative pairing and life history strategy - a cross-cultural study. {\em Human Nature}, 20:317--330.

\bibitem[2]{test2}
Another bibliography example.

\end{thebibliography}

%----------------------------------------------------------------------------------------
\end{document}
{% endhighlight %}
