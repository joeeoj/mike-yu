+++
title = "About Mike"
slug = "about"
+++

## Favorites

* Team: Mariners
* Bike manufacturer: Surly
* Text editor: Sublime Text
* Database engine: SQLite (maybe DuckDB soon)
* Python package: pandas
* Tech educator: Jake VanderPlas

## Frequently used Python patterns

Flatten nested lists:

```python
from itertools import chain

flattened = list(chain.from_iterable([[1, 2, 3], [4, 5, 6], [7, 8, 9]]))
```

csv.DictReader instead of csv.reader:

```python
with open('file.csv') as f:
    csvreader = csv.DictReader(f)
    for row in csvreader:
        val = row['col_name']
```

Turn a key/value list into dict of key -> list of values:

```python
from collections import defaultdict

d = defaultdict(list)
for row in rows:
    key, val = row
    d[key].append(val)
```

Parse a table on a website:

```python
import bs4
import requests

r = requests.get(url)
soup = bs4.BeautifulSoup(r.content, 'html.parser')
table = soup.find('table', id='table-hopefully-labeled-by-id')
rows = table.find_all('tr')

parsed_results = []
for row in rows[1:]:  # skip header
    cells = row.find_all('td')
```

Simplify columns in a pandas dataframe:

```python
import pandas as pd

df = pd.read_csv('file.csv')
df.columns = [c.strip().replace(' ', '_').lower() for c in df.columns]
```
