---
title: "Code Through Project - Fantasy Football Stats and Projections"
author: "Tod Kemper"
date: "`r format(Sys.time(), '%d %B, %Y')`"
output:
  html_document:
    df_print: paged
    theme: cerulean
    highlight: haddock
---
---

```{r setup, include=FALSE}
knitr::opts_chunk$set

```

## Summary about the package

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:

##Installing the package

We should first install the dependencies and the main package to be able to utilize the tools.

```{r install package}

install.packages(c("devtools","rstudioapi"), dependencies=TRUE, repos=c("http://rstudio.org/_packages", "http://cran.rstudio.com"))

devtools::install_github(repo = "FantasyFootballAnalytics/ffanalytics", build_vignettes = TRUE)

```

Once installed, we can load the package in any of our R projects.

```{r load libray}
library("ffanalytics")

```

## Scraping Fantasy Football Statistics

The main use for this tool is that it gathers data from many fantasy football websites and puts them into one dataset. This tool grabs from CBS, ESPN, or Yahoo, for example and you select the position or year. This time it was QBs, RBs, WRs, TEs, and DST for 201:

```{r scrape data}
my_scrape <- scrape_data(src = c("Yahoo"), 
                         pos = c("QB", "RB", "WR", "TE", "DST"),
                         season = 2019, week = 0)
```

##Calculating Projections

```{r project data}
my_projections <- projections_table(my_scrape)
```


##Player Data
Player data is scraped from MyFantasyLeague.com when the package loads and is stored in the player_table object.  To add player data to the projections table use the add_player_info() function, which adds the player names, teams, positions, age, and experience to the data set.

```{r player data}
my_projections <- my_projections %>% add_player_info()
```


##Displaying the Projections
By now you have run the scrape and saved the scraped data into an object and
you have calculated the projections using the scraped data. You can view the scraped data by typing, into the R console, the name of the object storing the scraped stats, along with position.  For example:

``` {r scraped stats}
my_scrape$RB
```

And you can view the projections by typing, into the R console, the name of the object storing the calculated projections.  For example:

``` {r project}

my_projections 
```


You can then save the projections by exporting them to .csv to be opened in Excel (you may have to change the path to a location where R has write privileges—R doesn’t have write privileges in the base c:/ directory):


To save the projections to your current working directory, use:
``` {r save locally}
write.csv(my_projections, file=file.path(getwd(), "projections.csv"), row.names=FALSE)
```
