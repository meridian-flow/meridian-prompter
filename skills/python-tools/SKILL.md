---
name: python-tools
description: >
  Patterns and references for writing Python scripts and tools. Load when
  writing standalone scripts, CLI tools, or utilities. Covers script structure,
  distribution patterns, and domain-specific references (scientific computing, etc.).
---

# Python Tools

When an agent isn't the right fit — the task is deterministic, a library solves the core problem, or you want reproducibility over flexibility — write a Python tool instead.

## When Tool > Agent

- Task is well-defined and deterministic
- A library already solves the core problem
- You want reproducibility (same input → same output)
- Cost matters (tools are free to run)

When uncertain, lean toward tools. You can add an agent layer later.

## Script Structure

```python
#!/usr/bin/env python3
"""
Brief description of what this tool does.

Usage:
    python scripts/my-tool.py <args>
"""

import argparse
from pathlib import Path


def main() -> int:
    parser = argparse.ArgumentParser(description=__doc__)
    parser.add_argument("input", type=Path, help="Input file")
    args = parser.parse_args()
    
    # Implementation
    
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

## Distribution

Where should the tool live?

| Location | When to use |
|----------|-------------|
| `scripts/` | Standalone scripts run directly |
| Entry point in `pyproject.toml` | Installable commands |
| Library module | Code other Python imports |

### Entry Points

```toml
[project.scripts]
my-tool = "package.module:main"
```

## Dependencies

Use `uv` for dependency management:

```bash
uv add <package>    # Add to project
uv sync             # Install dependencies
```

Never use `pip install` directly.

## Quality Bar

- **Works correctly** — produces expected output
- **Readable** — someone else can understand and modify it
- **Documented** — docstring explains usage and dependencies
- **Tested** — ran with sample data before reporting done

## Domain References

Load the relevant reference for specialized domains:

- `resources/scientific-python.md` — numpy, pandas, scipy, biomedical stack, conda/mamba setup
