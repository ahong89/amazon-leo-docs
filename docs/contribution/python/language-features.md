---
title: Language Features
sidebar_position: 3
---
# Language Features

## Dictionary Access
Accessing dictionary values: Always use [] syntax instead of .get(), unless you are using .get()’s optional argument to specify a default value

```python
value = data["key"]  # good
value = data.get("key")  # avoid (same behavior as above)
value = data.get("key", "default_value")  # good
```

## Typing Standards
1. Type hint all public functions and all FastAPI endpoints (inputs and outputs).
2. Use prebuilt generics for collections (e.g., `list[str]`, `dict[str, int]`).
3. Use `| None` for optional values (e.g., `str | None`)
4. Avoid `Any` unless it is required; add a short comment explaining why.
5. Use `TypedDict` or `dataclass` when a dictionary object has a fixed shape.
6. Prefer `Literal` for constrained string values.

```python
from dataclasses import dataclass
from typing import Literal, TypedDict

Status = Literal["pending", "active", "disabled"]

class UserPayload(TypedDict):
    name: str
    status: Status

@dataclass
class User:
    name: str
    status: Status
```
