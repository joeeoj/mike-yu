+++
title = "Projects"
slug = "projects"
+++

## fantasy football

![fantasy football charts](/projects/fantasy_football_2021_combined_charts_50pct.png)

https://github.com/joeeoj/fantasy-football ([nbviewer link](https://nbviewer.org/github/joeeoj/fantasy-football/tree/main/) to render the Altair graphs)

This was a project to learn NFL fantasy football strategy and practice data wrangling and analysis. It gave me a chance to try out [Altair](https://altair-viz.github.io/) and test out mkdocs for writing [project documentation](https://joeeoj.github.io/fantasy-football/) (using this [great tutorial by calmcode](https://calmcode.io/mkdocs/intro-to-mkdocs.html)).

2021/2022 results: 3rd place -- not bad considering I got second to last the year before (I had no idea what I was doing that year). Like most people, my biggest area for improvement is evaluating and making trades.

## dolthub data bounties

![us-jail code parsing example](/projects/jail_excerpt_50pct.png)

Dolt is Git + MySQL. The company that created it, [Dolthub](https://www.dolthub.com/), hosts data bounties to incentivize contribution to datasets. It has given me an opportunity to practice scraping websites with Python, pandas data manipulation, and SQL. Here are the data bounties I've contributed too:

* [hospital price transparency](https://www.dolthub.com/repositories/dolthub/hospital-price-transparency-v3)
* [us jails](https://www.dolthub.com/repositories/dolthub/us-jails)

## recipes

![whatsfordinner screenshot](/projects/whatsfordinner.png)

http://whatsfordinner.recipes/

A static website hosted by Netlify with my favorite recipes and some editorial notes. Great for last minute dinner planning. All the credit goes to [jeffThompson](https://github.com/jeffThompson) as this is based on his original [Recipes](https://github.com/jeffThompson/Recipes) website with modifications by [kvpsky](https://github.com/kvpsky). This falls well within the free tier of Netlify so the only cost was the domain for about $10. Adding new recipes is as simple as adding markdown pages, regenerating the html, and pushing changes to main ([repo](https://github.com/joeeoj/recipes)).

## mlbcal

![MLB schedule endpoint screenshot](/projects/mlb_schedule_endpoint_50pct.png)

https://github.com/joeeoj/mlbcal

A CLI wrapper around the MLB Stats API schedule endpoint to generate csv and json reports of a team's schedule. Publishing this to PyPI helped me better understand Python packaging (pro tip: TestPyPI is great, use it!).

Example usage:

`$ mlbcal seattle --csv --nopre > mariners_schedule.csv`

## lahnman-to-duckdb

![longest tenure player by team example query](/projects/longest_tenure_query_70pct.png)

https://github.com/joeeoj/lahnman-to-duckdb

A wrapper around the [baseballdatabank](https://github.com/chadwickbureau/baseballdatabank) (aka Lahnman database) to convert the CSV files into a [DuckDB](https://duckdb.org/) database for faster, local analytics of baseball data. I heard about DuckDB in a [Data Engineering Podcast](https://www.dataengineeringpodcast.com/duckdb-in-process-olap-database-episode-270/) episode. It is kind of like the OLAP version of SQLite.
