# Annotated (Type Metadata System)

`Annotated` is a Python typing feature used to attach **metadata** to a type.

It does NOT change runtime behavior by itself.

---

## Basic Syntax

```python
from typing import Annotated

UserId = Annotated[str, "from header"]
```

---

## Core Rules

- **Multiple Metadata:** `Annotated[int, "meta1", "meta2"]`
- **Type Checkers:** Treat `Annotated[T, meta]` simply as `T`.
- **Flattening:** `Annotated[Annotated[int, "a"], "b"]` equals `Annotated[int, "a", "b"]`.

---

## Common Use Cases

### FastAPI (Validation & Dependencies)
```python
from fastapi import Query, Depends
from typing import Annotated

# Validation
def get_items(q: Annotated[str, Query(min_length=3)]): ...

# Dependency Injection
def get_user(user: Annotated[User, Depends(get_current_user)]): ...
```

### Pydantic / SQLModel (Constraints)
```python
from pydantic import Field
from typing import Annotated

class User(BaseModel):
    name: Annotated[str, Field(min_length=1, max_length=50)]
```

---

## Extracting Metadata at Runtime

Use `get_type_hints` with `include_extras=True`.

```python
from typing import get_type_hints

def my_func(user_id: Annotated[str, "from_header"]): pass

hints = get_type_hints(my_func, include_extras=True)
metadata = hints["user_id"].__metadata__ 
# Output: ('from_header',)
```