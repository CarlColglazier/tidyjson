---
output: github_document
---

<!-- README.md is generated from README.Rmd. Please edit that file -->



# tidyjson

[![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/tidyjson)](http://cran.r-project.org/package=tidyjson)
[![Build Status](https://travis-ci.org/jeremystan/tidyjson.svg?branch=master)](https://travis-ci.org/jeremystan/tidyjson)
[![Coverage Status](https://img.shields.io/codecov/c/github/jeremystan/tidyjson/master.svg)](https://codecov.io/github/jeremystan/tidyjson?branch=master)

![tidyjson graphs](https://cloud.githubusercontent.com/assets/2284427/18217882/1b3b2db4-7114-11e6-8ba3-07938f1db9af.png)

tidyjson provides tools for [tidying](https://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html)
[json](http://www.json.org/).

## Installation

Get the released version from CRAN:

```R
install.packages("tidyjson")
```

Or the development version from github with:

```R
# install.packages("tidyjson")
devtools::install_github("jeremystan/tidyjson")
```

## Examples

The following example takes a character vector of 
500 
documents in the `worldbank` dataset and spreads out all objects into new 
columns


```r
library(tidyjson)
library(tibble)

worldbank %>% spread_all %>% tbl_df
#> Error in function_list[[k]](value): could not find function "tbl_df"
```

However, some objects in `worldbank` are arrays, this example shows how
to quickly summarize the top level structure of a JSON collection


```r
worldbank %>% gather_keys %>% json_types %>% count(key, type)
#> Error in function_list[[k]](value): could not find function "count"
```

In order to capture the data in `majorsector_percent` we can use `enter_object` 
to drop into that object, `gather_array` to stack the array and `spread_all`
to capture the object keys under the array.


```r
worldbank %>%
  enter_object("majorsector_percent") %>%
  gather_array %>%
  spread_all %>%
  tbl_df
#> Error in function_list[[k]](value): could not find function "tbl_df"
```

## API

### Spreading JSON objects into new columns

* `spread_all()` for spreading all object values into new columns, with nested
objects having keys like `parent.child`

* `spread_values()` for specifying a subset of object values to spread into
new columns using the `jstring()`, `jnumber()` and `jlogical()` functions

### Object navigation

* `enter_object()` for entering into an object by name, discarding all other
JSON and allowing further operations on the object value

* `gather_keys()` for stacking all object values by key name, expanding the
rows of the `tbl_json` object accordingly

### Array navigation

* `gather_array()` for stacking all array values by index, expanding the
rows of the `tbl_json` object accordingly

### JSON inspection

* `json_types()` for identifying JSON data types

* `json_length()` for computing the length of JSON data (can be larger than
`1` for objects and arrays)

* `json_complexity()` for computing the length of the unnested JSON, i.e.,
how many terminal leaves there are in a complex JSON structure

### JSON summarization

* `json_structure()` for creating a single fixed column data.frame that 
recursively structures arbitrary JSON data

* `json_schema()` for representing the schema of complex JSON, unioned across
disparate JSON documents, and collapsing arrays to their most complex type
representation

### JSON plotting

* `plot_json_graph()` for plotting JSON (or `json_schema()`) as a graph using
the `igraph` package

### Creating tbl_json objects

* `as.tbl_json()` for converting a string or character vector into a `tbl_json`
object, or for converting a `data.frame` with a JSON column using the
`json.column` argument

* `tbl_json()` for combining a `data.frame` and associated `list` derived
from JSON data into a `tbl_json` object

* `read_json()` for reading JSON data from a file

### Included JSON data

* `commits`: commit data for the dplyr repo from github API

* `issues`: issue data for the dplyr repo from github API

* `worldbank`: world bank funded projects from 
[jsonstudio](http://jsonstudio.com/resources/)

* `companies`: startup company data from 
[jsonstudio](http://jsonstudio.com/resources/)

## Philosophy

## Related Work

