# Package Info

A small Python toolkit for introspecting unfamiliar packages — extracting their functions, classes, methods, and attributes into a structured, filterable form, and visualizing how their parameters relate to one another. It is packaged as a single, self-contained [Jupyter notebook](package_info.ipynb) built around four functions.

Typical use cases include:

* Getting up to speed with an unfamiliar or large third-party library
* Discovering functions that share a given parameter or naming pattern
* Comparing the parameter signatures of related functions to spot common conventions
* Collecting the full set of documentation links for a package's website

## Functions

### `collect_package_info(package, tostring=False, sep='\t', include_private=False)`

Recursively walks a given (already-imported) package and its submodules, extracting every function, class, method, and attribute it finds, along with its signature and the first line of its docstring. Returns a list of dictionaries — ready to be turned into a [pandas](https://pandas.pydata.org/) DataFrame for filtering and analysis — or, if `tostring=True`, a tab-separated table.

### `extract_parameter_list(df, union=True, values=False)`

Given a (typically filtered) DataFrame produced by `collect_package_info`, extracts the set of parameter names used across the selected functions: their union (`union=True`, the default) or their intersection (`union=False`). Setting `values=True` retains parameter default values alongside their names.

### `plot_parameter_matrix(df)`

Builds a binary heatmap showing which of the selected functions accept which parameters, making it easy to spot shared arguments and design conventions across a set of related functions.

### `collect_links(root_url, max_pages=1000, output_file=None, parser='html.parser', links=None, delay=0)`

Crawls a website starting from `root_url`, collecting every link within the same domain — useful for gathering the full set of documentation pages for a package. Returns a list of links, or writes them to `output_file` if one is given.

Together, these tools make it faster to explore an unfamiliar codebase, identify reusable functionality, and understand how a package's components relate to each other — whether by introspecting the code directly or by mapping out its online documentation.

## Illustration

The notebook illustrates all four functions with [Seaborn](https://seaborn.pydata.org/) as the target package. It:

1. Runs `collect_package_info` on `seaborn` and loads the result into a DataFrame.
2. Filters that DataFrame down to top-level plotting functions ending in `plot` that accept a `data` argument.
3. Uses `extract_parameter_list` to compare the parameters common to Seaborn's `relational` plotting functions against the full set used across them.
4. Uses `plot_parameter_matrix` to visualize that same parameter presence as a heatmap.
5. Uses `collect_links` to gather documentation links from `https://seaborn.pydata.org/`.

## Requirements

The notebook relies on [pandas](https://pandas.pydata.org/), [Matplotlib](https://matplotlib.org/), [Requests](https://requests.readthedocs.io/), and [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/) (`bs4`), alongside Python's standard library (`inspect`, `pkgutil`, `importlib`, etc.). All are preinstalled in [Google Colab](https://colab.research.google.com/) except `bs4`, which the notebook installs as needed. The illustration additionally uses [Seaborn](https://seaborn.pydata.org/) as the package being introspected.

## Running the notebook

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dimitrisilva/package-info/blob/main/package_info.ipynb)

The notebook can also be run locally in Jupyter, provided the packages above are installed.
