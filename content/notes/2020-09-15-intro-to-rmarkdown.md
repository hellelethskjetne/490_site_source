---
title: Intro to RMarkdown
author: ~
date: '2020-09-15'
slug: intro-to-rmarkdown
categories: [rmarkdown]
tags: [week05]
draft: false
---

Intro to the RMarkdown + knitr system for creating reproducible documents.ß

<!--more-->

This week we will begin working in RMarkdown. 

RMarkdown is two tools wrapped into one:

* **markdown**, a markup language that allows you to easily create formatted documents like web pages from plain text documents
* **knitr**, a tool that executes R code and integrates the results with markdown text.

Together, these allow us to write nicely-formatted documents that mix text with R code, and the results of the R code. The resulting documents are very reproducible, in the sense that we see exactly how the raw data get turned into results. Additionally, the fact that the RMarkdown source document (.Rmd file) is just a plain text file means that we can use version control to track changes in our document and collaborate with others. [Pat Schloss' lab](https://twitter.com/PatSchloss/status/1305551736359223296) writes all of their documents in RMarkdown!

I won't try to recapitulate the excellent overview of RMarkdown that RStudio has put together [here](https://rmarkdown.rstudio.com/) or the RMarkdown [chapter](https://r4ds.had.co.nz/r-markdown.html) in Grolemund and Wickham. Please go read those now.

Done reading? Good. I want to highlight a couple of points about RMarkdown:

* The way the RMarkdown system in RStudio works is that. when we press "knit":
  * RStudio launches a new R session to evaluate the R code
  * **knitr** creates a markdown file that contains the markdown text we wrote plus the code we wrote and the output from the code we wrote.
  * RStudio uses a tool called [pandoc](https://pandoc.org/) to convert the markdown file into whatever type of output file we specify in the RMarkdown header. Most often this will be an HTML file (web page), a PDF file, or a Word document.
* It turns out that GitHub knows how to interpret a specific 'flavor' of markdown called "GitHub-flavored markdown". Since you'll be posting your homework to GitHub, you should output GitHub-flavored markdown files. To do that, put the following in your RMarkdown file header: `output: github_document`. 
* RStudio creates a **new R session** to run your code. That has a couple of implications:
  * Any objects you may have created in your global environment (i.e., what you type at the console) won't be available to the R session that executes the code in your RMarkdown document. Similarly, you have to load all the packages you need in the RMarkdown document itself. **An RMarkdown document must be entirely self-contained**.
  * The R session that executes the code uses, as its working directory, whatever directory the .Rmd file is saved in. Imagine you are working in an RStudio Project called `reproducible_data_analysis`, which is structured like this:
  ```
  reproducible_data_analysis
    |--data
        |--my_data.csv
    |--reports
        |--my_report.Rmd
  ```
  This is a nice logical way to structure your directory, but since the `my_report.Rmd` file is saved in `reproducible_data_analysis/reports`, the working directory for the R code in your `my_report.Rmd` will be `reproducible_data_analysis/reports`. However, as you write the code, you will probably be executing it line-by-line. When you execute the code at the console, the working directory will be `reproducible_data_analysis`. So if you want to read in `my_data.csv` from the console, you might use `read_csv("data/my_data.csv")`. But when you read it in when you knit the .Rmd, you would use the path `"../data/my_data.csv"`.



* Remember that the R code in your RMarkdown document runs in a different R session than what you see in the console. That means that the R code in your R Markdown document needs to be totally self-contained - objects you define in the console will not be available to the R session that creates the document. (This is a good thing: it forces the document to be more reproducible.)