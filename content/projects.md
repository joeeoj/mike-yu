+++
title = "Projects"
slug = "projects"
+++

## dolthub data bounties

Dolt is Git + MySQL. The company that created it, [Dolthub](https://www.dolthub.com/), hosts data bounties to incentivize contribution to certain datasets. It has given me an opportunity to practice a variety of skills such as: Python scripting to scrap data from websites, pandas data manipulation, and writing SQL queries.

Here are the ones I've contributed to so far:

* [hospital price transparency](https://github.com/joeeoj/hospital-price-transparency-v3) ([scoreboard](https://www.dolthub.com/repositories/dolthub/hospital-price-transparency-v3/bounties/00b14320-da0f-4374-9c40-73f3ed302aaf/scoreboard))
* [us jails](https://github.com/joeeoj/us-jails)

## mlbcal

https://github.com/joeeoj/mlbcal

A pip-installable project that is a CLI wrapper around the MLB Stats API schedule endpoint.

Example usage:

`$ mlbcal seattle --csv --nopre > mariners_schedule.csv`

## lahnman-to-duckdb

https://github.com/joeeoj/lahnman-to-duckdb

A wrapper around the [baseballdatabank](https://github.com/chadwickbureau/baseballdatabank) (aka Lahnman database) to convert the CSV files into a [DuckDB](https://duckdb.org/) database for faster, local analytics of baseball data. DuckDB is kind of like the OLAP version of SQLite.

Example queries against the database can be found here: [examples.ipynb](https://nbviewer.org/github/joeeoj/lahnman-to-duckdb/blob/main/src/analysis/examples.ipynb)

## recipes

http://whatsfordinner.recipes/

A static website, hosted by Netlify, with my favorite recipes and some editorial notes. Great for last minute dinner planning. Based on [Recipes](https://github.com/jeffThompson/Recipes) by [jeffThompson](https://github.com/jeffThompson) with modifications by [kvpsky](https://github.com/kvpsky).

## fantasy football

https://github.com/joeeoj/fantasy-football

Project to learn NFL fantasy football strategy while practicing analysis and engineering skills.

2021-2022 season results: 3rd place
