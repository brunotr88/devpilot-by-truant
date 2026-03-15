# Anti-Patterns Library

Common code anti-patterns with examples of bad and good implementations.

## God Class
```python
# BAD — one class doing everything
class UserManager:
    def create_user(self): ...
    def send_email(self): ...
    def process_payment(self): ...
    def generate_report(self): ...
    def validate_input(self): ...

# GOOD — single responsibility
class UserService:
    def create_user(self): ...

class EmailService:
    def send_email(self): ...

class PaymentService:
    def process_payment(self): ...
```

## Deep Nesting
```python
# BAD
if condition1:
    if condition2:
        if condition3:
            if condition4:
                do_something()

# GOOD — early returns
if not condition1:
    return
if not condition2:
    return
if not condition3:
    return
if not condition4:
    return
do_something()
```

## Magic Numbers
```python
# BAD
if user.age > 18:
    grant_access()
if retry_count > 3:
    raise Exception("Failed")

# GOOD
MINIMUM_AGE = 18
MAX_RETRIES = 3

if user.age > MINIMUM_AGE:
    grant_access()
if retry_count > MAX_RETRIES:
    raise RetryExhaustedError(f"Failed after {MAX_RETRIES} attempts")
```

## Premature Abstraction
```python
# BAD — abstraction for one use case
class AbstractDataProcessor:
    def process(self): ...
class ConcreteDataProcessor(AbstractDataProcessor):
    def process(self): ...

# GOOD — just write the code
def process_data(data):
    # Simple, direct implementation
    return transformed_data
```

## Boolean Blindness
```python
# BAD
def process(data, flag1, flag2, flag3):
    if flag1 and not flag2 and flag3:
        ...

# GOOD — use enums or named parameters
class ProcessMode(Enum):
    STRICT = auto()
    LENIENT = auto()
    DRY_RUN = auto()

def process(data, mode: ProcessMode):
    ...
```

## Error Swallowing
```python
# BAD — silent failure
try:
    important_operation()
except Exception:
    pass

# GOOD — handle or propagate
try:
    important_operation()
except SpecificError as e:
    logger.error(f"Operation failed: {e}")
    raise
```

## Stringly Typed
```python
# BAD
def set_status(status: str):
    if status == "active":
        ...
    elif status == "inactive":
        ...

# GOOD
class Status(Enum):
    ACTIVE = "active"
    INACTIVE = "inactive"

def set_status(status: Status):
    ...
```
