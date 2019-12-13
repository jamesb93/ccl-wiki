# Data handling for visualisation computational musicology - first ideas

### [Laurens van der Wee](https://www.kelyje.nl)
---
# Introduction

Jacob Hart and I are working on visualisation and interaction for computational musicology purposes. This happens on the cross-section of the IRiMaS and FluCoMa projects, incorporating ideas, code and strategies from both.
In the HackSpace session, we spoke mostly about the database functionality. For context: we work from within Max 8, but may want to open this up to other languages and software applications, so generic formats and exporting functions are crucial.

## Data

What we're doing: generating data in a multitude of ways and then using this data for a variety of visualisations. Since these are not aligned or we don't know beforehand what we may need later on, we want a flexible underlying system to deal with data.
A database seems the right option. I've presented and am still working on a Javascript file that can be required by other javascripts to populate databases, change data in them or read from them. All based on this tutorial that lays down the basics for SQLite in Max: https://cycling74.com/tutorials/data-collection-building-databases-using-sqlite.

This way we get:
- easy access
- easy read
- flexibility in terms of structure
- portability: it works on a file
- ...

Other objects/structures we have looked at:
- entrymatcher by Alex Harker: although very nice (the unreleased new build), it's quite limited in the way one can query and it's not flexible 
- dict: more or less workable when used from Javascript, but also not easily queryable in some situations and a nightmare when used in a Max patch without Javascript
- ...

## Future

If thoroughly tested and more or less complete, the script will be released in the public domain.
