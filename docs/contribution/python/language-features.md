---
title: Language Features
sidebar_position: 3
---
# Language Features

## Dictionary Access
Accessing dictionary values: Always use `[]` syntax instead of `.get()`, unless you are using
`.get()`’s optional argument to specify a default value.

```python
value = data["key"]  # good
value = data.get("key")  # avoid (same behavior as above)
value = data.get("key", "default_value")  # good
```

## Typing Standards
1. Type hint all public functions and all FastAPI endpoints (inputs and outputs).
```python
async def create_shapefile(self, latitude: float, longitude: float) -> str:
```
2. Use prebuilt generics for collections (e.g., `list[str]`, `dict[str, int]`).
```python
from typing import Dict
def xyz() -> Dict[str, int]: # bad, comes from typing module

def xyz() -> dict[str, int]: # good, uses generics
```
3. Use `| None` for optional values (e.g., `str | None`) as opposed to `Optional[str]` (outdated syntax)
```python
simplify: float | None = None
```
4. Avoid `Any` unless it is required; add a short comment explaining why.
5. Use `pydantic` models when a dictionary object has a fixed shape.
```python
from pydantic import BaseModel
class CreateSessionOut(BaseModel):
    session_id: str
    shapefiles: list[str]
```
6. Prefer `Literal` for constrained string values.

```python
from typing import Literal
Status = Literal["pending", "active", "disabled"]
```
