---
title: R, Excel and PostgreSQL
subtitle: "Using R to get out of a corporate timesuck :cat:"
author: ~
date: '2018-12-07'
slug: r-excel-and-postgresql
categories: ["R"]
tags: ["R Markdown", "RStudio", "Education"]
---

I deal with a lot of numerical data at work, and because it's a corporate environment, that data comes more often than not in the form of an .xlsx or .csv file. As it gets passed around, it's manipulated to the point that it can no longer be trusted in terms of content. Creating the original file is also a very manual process. The less human-guided work which needs to be done to get a forecast or analysis file production ready.

# The Current Workflow

Over the course of 10 minutes I'll pull down several large datasets using queries against a PostgreSQL database. It aggregates a lot of data but the output is confusing if you haven't had it explained before. I'll then spend the next 45 minutes with a previous analysis on one screen and the raw data on the other, rebuilding formulas and calculations to get the forecast into its preferred state. If a novel analysis is needed, add a few hours to figure out how to trick Excel into doing what you'd like it to do.

When I'm done, I've got a 10 megabyte excel file with almost 150 columns with, as an example, only the last 12 columns or so representing what I want: a rolling 12 month forecast. I then roll a d20 to see if the file crashes before I can save the final version. <i class="fas fa-dice-d20"></i>

The SQL query above consists of about 10-15 left joins and "pivoted" queries. I take dated transactional data, summarize it by month and then use ```CASE ... END``` to get the numbers into the correct columns. I recognize that you can use a dynamic method to create a true pivot but it's overkill when all I want to do is pivot a year's worth of transactional history and have the columns be month names relative to the current month.

Between this ugly SQL manipulation and the time it takes to just build the excel formulas, there had to be a better way.

# R to the Rescue!

I've been using R for about a year, but it wasn't until JD Long's talk at the NorEast'R Conference about corporate reporting that I thought to use at work. I had never used R for operation against an actual database before, instead dumping data to CSV, but I wanted to eliminate every manual step possible.

## Connect to PostgreSQL

At the time of writing this, I found two easy ways to connect to PostgreSQL through R. This assumes that you're server is publicly accessible or you're on the same side of the corporate firewall / using a VPN.

```
I am
a great
code
example
dirrrr
```

## Query Transactional Data
## Pivot Within R
## Join Along Common Key
## Add Excel Formulas
## Output To Excel