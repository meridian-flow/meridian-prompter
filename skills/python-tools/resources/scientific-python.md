# Scientific Python

Reference for data analysis and scientific computing scripts.

## Stack

Common libraries by domain:

- **Data manipulation** — pandas, polars
- **Numerical** — numpy, scipy
- **Visualization** — matplotlib, seaborn, plotly
- **Machine learning** — scikit-learn, xgboost
- **Biomedical** — biopython, scanpy, anndata
- **Statistics** — statsmodels, pingouin

## Package Manager

Scientific packages often have native dependencies. Check what's available:

```bash
which mamba conda uv pip
```

Preference order: **mamba > conda > uv > pip**

Conda/mamba handle packages with C extensions, BLAS dependencies, etc. better than pip. For pure Python packages, uv is faster.

## Script Shape

Single-file `.py` scripts saved to workspace:

```python
"""
Descriptive title.

Usage:
    python script_name.py input.csv --output results.csv

Dependencies:
    pip install pandas matplotlib
"""

import argparse
import pandas as pd

def main():
    parser = argparse.ArgumentParser(description="What this does")
    parser.add_argument("input", help="Input file")
    parser.add_argument("--output", default="output.csv", help="Output file")
    args = parser.parse_args()
    
    # Core logic here
    
if __name__ == "__main__":
    main()
```

## Quality Bar

- **Works correctly** — produces expected output
- **Readable** — someone else can understand and modify it
- **Documented** — docstring explains usage and dependencies
- **Tested** — ran with sample data before reporting done
