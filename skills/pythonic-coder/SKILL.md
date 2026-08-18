---
name: pythonic-coder
description: >
  Writes, reviews, refactors, and tests idiomatic, maintainable Python: naming, function design, type hints, error
  handling, resource management, imports, project structure, logging, and testing. Use whenever creating or reviewing
  Python modules, scripts, libraries, services, data pipelines, CLIs, or tests, or when asked to make Python code more
  "pythonic," clean up a script, add type hints, or refactor existing Python.
license: MIT
source: https://realpython.com/tutorials/best-practices/
version: 1.0.0
author: ByteMeDirk
---

# Pythonic Code Skill

## Purpose

Produce Python that is easy to read, safe to change, straightforward to test, and unsurprising to experienced Python
developers.

Prioritize this order:

1. Correctness
2. Clarity
3. Maintainability
4. Testability
5. Performance, after measurement

Prefer explicit, boring, well-named code over compressed, clever code.

## Core principles

- Follow the spirit of the Zen of Python: readable, explicit, simple, and practical.
- Keep each function and module focused on one responsibility.
- Use Python standard-library features before adding dependencies.
- Make invalid states difficult to represent.
- Separate business logic from I/O, framework glue, configuration, and infrastructure.
- Write code for the next engineer reading it, not merely for the interpreter.
- Use type hints where they make inputs, outputs, and contracts clearer.
- Raise meaningful errors close to the source of a failure.
- Test behaviour rather than implementation details.

## Naming

Use names that describe purpose and units.

```python
# Good
retry_count = 3
timeout_seconds = 30
active_user_ids = get_active_user_ids()

# Avoid
x = 3
t = 30
data = get_data()
```

Follow standard naming conventions:

- Modules and packages: `snake_case`
- Functions and variables: `snake_case`
- Classes: `PascalCase`
- Constants: `UPPER_SNAKE_CASE`
- Type variables: short but conventional, such as `T`, `T_co`, or `KeyT`
- Private implementation details: prefix with a single underscore, for example `_parse_row`

Use verbs for functions that perform actions:

```python
def load_configuration(path: Path) -> AppConfig:
    ...
```

Use nouns or adjective phrases for values and predicates:

```python
is_valid = validate_email(email)
user_count = len(users)
```

Avoid unclear abbreviations unless they are widely understood in the domain, such as `id`, `url`, `sql`, or `api`.

## Functions

Keep functions small, cohesive, and easy to describe in one sentence.

A function should:

- Have a clear name.
- Do one logical job.
- Accept only the inputs it needs.
- Return a clear value or raise a meaningful exception.
- Avoid hidden mutation and hidden I/O where practical.

Prefer guard clauses over deeply nested conditionals:

```python
def calculate_discount(customer: Customer, amount: Decimal) -> Decimal:
    if amount <= 0:
        raise ValueError("amount must be greater than zero")

    if not customer.is_eligible_for_discount:
        return Decimal("0")

    return amount * customer.discount_rate
```

Avoid boolean flag arguments that switch a function between unrelated behaviours:

```python
# Avoid
def export_report(report: Report, as_csv: bool) -> str:
    ...


# Prefer
def export_report_as_csv(report: Report) -> str:
    ...


def export_report_as_json(report: Report) -> str:
    ...
```

Prefer returning values over printing from reusable functions. Keep `print()`, logging setup, HTTP requests, database
access, filesystem access, and CLI argument parsing at the application boundary.

## Data structures and idioms

Choose the simplest structure that expresses the domain.

- Use `list` for ordered, mutable sequences.
- Use `tuple` for fixed-size records or immutable sequences.
- Use `set` for membership tests and uniqueness.
- Use `dict` for key-value lookup.
- Use `dataclass` for simple domain objects with data and lightweight behaviour.
- Use `Enum` when values form a fixed, named set.
- Use `pathlib.Path` instead of manipulating filesystem paths as strings.
- Use `collections` types such as `Counter`, `defaultdict`, and `deque` when they fit naturally.

Use comprehensions for simple transformations and filtering:

```python
active_names = [
    user.name
    for user in users
    if user.is_active
]
```

Use a regular loop when the operation has multiple steps, side effects, error handling, or complex conditions:

```python
active_names: list[str] = []

for user in users:
    if not user.is_active:
        continue

    normalized_name = normalize_name(user.name)
    active_names.append(normalized_name)
```

Use unpacking where it improves clarity:

```python
first_name, last_name = full_name.split(maxsplit=1)
```

Use `enumerate()` rather than manually incrementing an index:

```python
for index, item in enumerate(items, start=1):
    process(index, item)
```

Use `zip(..., strict=True)` when paired iterables must have equal length:

```python
for user_id, email in zip(user_ids, emails, strict=True):
    update_email(user_id, email)
```

Use `any()` and `all()` for clear boolean aggregation:

```python
has_errors = any(result.is_error for result in results)
all_valid = all(record.is_valid for record in records)
```

## Type hints

Use modern Python type syntax when supported by the project version.

```python
def find_user(user_id: str) -> User | None:
    ...


def group_events(events: list[Event]) -> dict[str, list[Event]]:
    ...
```

Use types to clarify public contracts and non-obvious values, especially:

- Function parameters and return types.
- Public classes and methods.
- Complex dictionaries and nested collections.
- Boundaries between modules or services.
- Parsed configuration and external payloads.

Prefer domain models over loosely typed dictionaries:

```python
from dataclasses import dataclass


@dataclass(frozen=True, slots=True)
class Customer:
    customer_id: str
    email: str
    is_active: bool
```

Avoid using `Any` unless interacting with genuinely dynamic or untyped external data. Narrow external data promptly
through parsing and validation.

```python
def parse_event(payload: object) -> Event:
    if not isinstance(payload, dict):
        raise TypeError("event payload must be a dictionary")

    event_type = payload.get("event_type")
    if not isinstance(event_type, str):
        raise ValueError("event payload requires a string event_type")

    return Event(event_type=event_type)
```

Use `Protocol` when code depends on behaviour rather than a concrete class.

```python
from typing import Protocol


class SupportsSend(Protocol):
    def send(self, message: str) -> None:
        ...
```

## Error handling

Catch only exceptions that can be handled meaningfully.

```python
try:
    settings = load_settings(config_path)
except FileNotFoundError as error:
    raise ConfigurationError(
        f"Configuration file does not exist: {config_path}"
    ) from error
```

Rules:

- Catch specific exception classes, never bare `except:`.
- Do not silently ignore failures.
- Add context when re-raising errors using `raise ... from error`.
- Use custom exception classes for meaningful domain failures.
- Use `ValueError` for invalid values and `TypeError` for invalid types when appropriate.
- Validate inputs at system boundaries.
- Do not use exceptions for ordinary branching logic.

Prefer EAFP when an operation is naturally exception-based:

```python
try:
    value = mapping[key]
except KeyError:
    value = default_value
```

Prefer explicit checks when they communicate intent more clearly or avoid expensive failures:

```python
if not file_path.exists():
    raise FileNotFoundError(file_path)
```

## Resource management

Use context managers for resources that need deterministic cleanup:

```python
from pathlib import Path


def read_text(path: Path) -> str:
    with path.open(encoding="utf-8") as file:
        return file.read()
```

Use `with` for files, locks, database connections, transactions, temporary resources, and similar lifecycle-bound
objects.

Do not rely on garbage collection to close resources.

## Imports

Organize imports in this order, separated by blank lines:

1. Python standard library
2. Third-party packages
3. Local application imports

```python
from pathlib import Path
from typing import Iterable

import httpx

from app.models import User
from app.services.users import UserService
```

Rules:

- Prefer absolute imports.
- Do not use wildcard imports.
- Import only what is needed.
- Avoid circular imports by improving module boundaries.
- Keep optional or expensive imports local only when justified and documented.

## Modules and project structure

Organize code by responsibility and domain, not by arbitrary file size.

Recommended layout:

```text
project/
├── pyproject.toml
├── README.md
├── .gitignore
├── src/
│   └── package_name/
│       ├── __init__.py
│       ├── domain/
│       ├── services/
│       ├── infrastructure/
│       └── cli.py
└── tests/
    ├── unit/
    └── integration/
```

Keep these concerns separate:

- Domain logic: rules, models, transformations.
- Application services: orchestration and use cases.
- Infrastructure: databases, APIs, filesystems, queues, cloud SDKs.
- Presentation and entry points: CLI, web endpoints, jobs, notebooks.
- Configuration: environment variables, config files, secrets integration.

Keep secrets out of source control. Read configuration from environment variables or approved secret-management systems,
validate it once at startup, and pass typed configuration into application components.

## Documentation

Write docstrings for public modules, classes, and functions where intent is not obvious from the signature and name.

Use docstrings to explain:

- Purpose.
- Important assumptions or invariants.
- Non-obvious side effects.
- Important exceptions.
- Semantics of parameters or return values when needed.

Do not write docstrings that merely repeat the function name.

```python
def parse_iso_date(value: str) -> date:
    """Parse a calendar date in ISO 8601 `YYYY-MM-DD` format.

    Raises:
        ValueError: If `value` is not a valid ISO calendar date.
    """
    return date.fromisoformat(value)
```

Use comments to explain *why*, not *what* obvious code does.

## Logging

Use the `logging` module in applications and libraries. Do not use `print()` for operational diagnostics.

```python
logger.info(
    "Imported customer records",
    extra={"record_count": len(records), "source": source_name},
)
```

Rules:

- Log meaningful events and operational context.
- Never log secrets, tokens, passwords, private keys, or unnecessary personal data.
- Use appropriate levels: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`.
- Let the application entry point configure logging.
- Preserve exception tracebacks with `logger.exception()` inside exception handlers.

## Testing

Write tests alongside implementation changes.

Use `pytest` and structure tests around observable behaviour.

```python
def test_calculate_discount_returns_zero_for_ineligible_customer() -> None:
    customer = Customer(
        customer_id="cust-123",
        email="user@example.com",
        is_active=True,
        is_eligible_for_discount=False,
        discount_rate=Decimal("0.10"),
    )

    discount = calculate_discount(customer, Decimal("100.00"))

    assert discount == Decimal("0")
```

Test these cases when relevant:

- Expected success path.
- Boundary values.
- Invalid input.
- Empty input.
- Failure and recovery paths.
- Regression cases for fixed bugs.

Prefer:

- Small deterministic unit tests.
- Fixtures that make setup readable.
- Dependency injection for external services.
- Temporary directories and fakes for filesystem or network interactions.
- Integration tests for real boundaries, run separately when costly.

Avoid:

- Tests dependent on execution order.
- Shared mutable global state.
- Real network calls in unit tests.
- Asserting private implementation details unless necessary.
- Excessive mocking that duplicates the implementation.

## Code quality tooling

Use automated tooling consistently.

Recommended commands:

```bash
ruff check .
ruff format --check .
mypy src
pytest
coverage run -m pytest
coverage report
```

Recommended `pyproject.toml` baseline:

```toml
[tool.ruff]
line-length = 88
target-version = "py312"

[tool.ruff.lint]
select = ["E", "F", "I", "B", "UP", "SIM"]

[tool.ruff.format]
quote-style = "double"

[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = "-ra"

[tool.mypy]
python_version = "3.12"
strict = true
mypy_path = "src"
```

Adapt the Python target version to the actual project. Do not enable strict typing mechanically in an existing untyped
codebase without planning incremental adoption.

Use pre-commit hooks to run formatting, linting, and basic validation before commits.

## Security and reliability

Treat all external input as untrusted, including API payloads, files, environment variables, command-line arguments,
database fields, and LLM output.

- Validate and normalize input at boundaries.
- Use parameterized queries; never construct SQL through string interpolation.
- Use `subprocess.run()` with argument lists and avoid `shell=True` unless unavoidable.
- Use safe parsers such as `json`; do not use `eval()` or unsafe deserialization.
- Set timeouts for network operations.
- Handle retries deliberately and only for transient, idempotent operations.
- Make side effects explicit and auditable.
- Avoid mutable default arguments.

```python
# Avoid
def add_item(item: str, items: list[str] = []) -> list[str]:
    items.append(item)
    return items


# Prefer
def add_item(item: str, items: list[str] | None = None) -> list[str]:
    result = [] if items is None else list(items)
    result.append(item)
    return result
```

## Refactoring checklist

When asked to improve existing Python code:

1. Preserve observable behaviour unless the request explicitly changes it.
2. Identify unclear names, mixed responsibilities, duplication, deep nesting, and hidden side effects.
3. Improve one concern at a time.
4. Add or update tests before making risky changes.
5. Replace complex conditionals with guard clauses where this improves readability.
6. Extract cohesive helpers, but do not create abstractions for a single trivial use.
7. Add type hints at public boundaries and around ambiguous data.
8. Run formatter, linter, type checker, and tests.
9. Explain any trade-offs, behavioural changes, or migration concerns.

## Agent delivery standard

When generating or modifying Python code:

- State assumptions if requirements are ambiguous.
- Prefer complete, runnable examples over isolated fragments when practical.
- Include imports needed by the code.
- Keep edits scoped to the request.
- Do not invent APIs, environment variables, database schema, or third-party library behaviour.
- Call out external dependencies and configuration requirements.
- Include tests for non-trivial logic.
- Provide commands to format, lint, type-check, and test changed code.
- If a requested approach is unpythonic or risky, explain the concern and offer a cleaner alternative.
- Before finalizing, review for naming, types, exception handling, resources, tests, security, and unnecessary
  complexity.

## Final review questions

Before considering work complete, verify:

- Is the code readable without a detailed explanation?
- Does every function have one clear responsibility?
- Are names specific and meaningful?
- Are types useful at important boundaries?
- Are errors actionable and handled at the right level?
- Are resources closed with context managers?
- Is configuration separate from application logic?
- Are external inputs validated?
- Are tests present for important behaviour and edge cases?
- Do formatting, linting, type checking, and tests pass?