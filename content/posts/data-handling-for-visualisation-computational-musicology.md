# Data handling for computational musicology and visualiation: early stages

### [Laurens van der Wee](https://www.kelyje.nl)
---
## Introduction

Jacob Hart and I have been working on visualisation and interaction for computational musicology. This happens at the cross-section of the [IRiMaS](https://research.hud.ac.uk/institutes-centres/irimas/) and [FluCoMa](http://www.flucoma.org) projects, incorporating ideas, code and strategies from both. In the most recent HackSpace session, we spoke mostly about the problem of implementing or choosing a database technology. For context: we work from within Max 8, but may want to open our tools to other languages and software applications, so generic formats and exporting functions are crucial to these efforts.

## Initial Experiments
---

Before looking to build our own technology to solve the problems of data storage and retrieval, we investigated existing solutions.

### `entrymatcher` by [Alex Harker](https://github.com/AlexHarker/AHarker_Externals): 

although `entrymatcher` ships with extremely rapid queries and functionality for easily shifting(especailly on the bleeding edge compiles), has an unsuitable querying language that is not suited to our research aims. Also, despite the existence of neat and concise C++ code to build into a Max object, something more application

### Max native `dict` object 

Max's native `dict` object can restore and store to a .json file which makes this a solid choice for interoperability. However, the interface for querying dict is cumbersome and makes it hard to develop your own interfaces that might support a more bespoke syntax for access. Dictionaries become much more usuable from within javascript in Max, but this adds another layer of abstraction for accessing the data.

## Data
---

What we're doing: generating data in a multitude of ways and then using this data for a variety of visualisations. Since these are not aligned or we don't know beforehand what we may need later on, we want a flexible underlying system to deal with data and potential metadata.
A database seems the right option. I've presented and am still working on a Javascript file that can be required by other javascripts to populate databases, change data in them or read from them. All based on [this tutorial](https://cycling74.com/tutorials/data-collection-building-databases-using-sqlite) that lays down the basics for SQLite in Max: 

By interfacing with SQLite through javascript we achieve:
- simple access methods with a high ceiling for complexity
- portability: the SQLite database is a file that can be parsed in other languages and environments
- simple read mechanisms, just point toward the entire database contained in a file

## Future
---

Once thoroughly tested our tools for computational musicology will be released into the public domain, most likely through github. As we are currently in the early development stages we are also interested in fielding any comments or insight into this area of research.