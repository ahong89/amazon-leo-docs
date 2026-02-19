---
title: Style
sidebar_position: 1
---
# Style

## Indentation and Spacing / Horizontal Spacing
Always use 4 space tabbing. (i.e. not `\t` character)  
Keep spacing around operators

```python
    sum = a + b  # good
    sum=a+b      # bad
```
Limit line length to be < 80 characters

## Commas
Always use trailing commas (results in consistent git merging behavior)

```python
data = {
    "key": 5,
    "key2": 4, # <-- this one
}
```

## Imports
Prefer absolute imports over relative imports when possible. Use relative imports only when the
path would be overly long.

```python
from app.file_name import xyz  # good
from ..file_name import xyz  # avoid when possible
```
