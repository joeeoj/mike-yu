+++
title = "About Mike"
slug = "about"
+++

## Fun facts

* Favorite book: Where the Wild Things Are
* Favorite beer: Bodhizafa IPA
* Once was chased by a moose in Alaska. Do not recommend.

## Career

```python
import json

with open('resume.json') as f:
    resume = json.load(f)

for (age, exp) in zip(range(21, 37, 3), resume):
    state, job = exp.get('state'), exp.get('job')
    print(f'{age} -> {state} | {job}')
```

```text
>>> 21 -> IL | Music Major
>>> 24 -> GA | Project Manager
>>> 27 -> WA | Business Analyst
>>> 30 -> WA | Solution Architect
>>> 33 -> WA | Senior Data Analyst
>>> 36 -> ?? | Data Engineer ?
```
