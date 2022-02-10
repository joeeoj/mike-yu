+++
title = "About Mike"
slug = "about"
+++

## Fun facts

* Currently Reading: My Year Abroad by Chang-rae Lee
* Favorite team: Mariners (see [The History of the Seattle Mariners](https://www.youtube.com/watch?v=TIgK56cAjfY))
* Famously featured in the [Embers Grille Big Triple Challenge](https://www.youtube.com/watch?v=3a4lG5SRLkE) commercial

## Career

```python
import json

with open('resume.json') as f:
    resume = json.load(f)

for exp in resume:
    print(f'{exp["state"]} -> {exp["job"]}')
```

```text
>>> IL -> Music Major
>>> GA -> Project Manager
>>> WA -> Business Analyst
>>> WA -> Solution Architect
>>> WA -> Data Analyst
>>> WA -> Senior Data Analyst
```
