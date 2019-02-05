---
title: 'Tableau Summary'
date: 2019-02-05
permalink: /posts/2019/02/blog-Tableau-1/
tags:
  - Tableau
  - Summary
---

(Summarized from toturial on Datacamp: https://www.datacamp.com/community/tutorials/data-visualisation-tableau)
# Tableau
## Terms 
**Dimensions**: qualitative data

**Measures**: quantitative data

**Aggregation**: row-leel data rolled up to a higher category, such as the sum of sales or total profit.

## View the data
Dray the variables to the column/row slot.


## Emphasizing the Results
### Adding filters to the  view
Right-click on variables
### Adding Colors to the view
Drag variable to Marks-Color

## Map View
Add `state` and `country` under data pane to `Detail` on the Marks card.

**Note**: In this part, we filter on `city` to show the the five cities with the most negative profit in the south region. But, the result shows six records, with the city `jacksonville` shown under Florida and North Caolina. The author said we could select other variables, such as `state` and `Add to Context` but still we can't get rid of the duplicated `jacksonville`. The author just `exclude` the unwanted record manually.

My understanding is that, when we filter `city`, we use it as an index, filter it to find the values corresponding to our selected condition(in this case, the five cities with smallest profit). Then Tableau use the values we get as the index and show the corresponding records. The problem is that `city` is not appropriate to be used as index, since it is not unique for each city. Instead I filter `Postal Code`, which is unique for each city, to get the results we want.



## Dashboard
_"A dashboard is a collection of several views, enabling one to compare a variety of data simultaneously."_

### Adding Interactiveness

## Story

## Tableau's integration with R, Python & SQL

### Tableau and R
Install in R
```
install.packages(“Rserve”)
library(Rserve)
Rserve() / Rserve(args = ‘ — no-save’)
```
In Tableau:
```
Help > Settings and Preferences and select Manage External Service Connection

Enter the server name as “Localhost” (or “127.0.0.1”) and a port of “6311”.
```

Four functions are available for use with R:
* SCRIPT_REAL: returns Real numbers as results
* SCRIPT_STR: returns string
* SCRIPT_INT : returns integers
* SCRIPT_BOOL: returns booleans

e.g.
```
SCRIPT_REAL("
fit <- lm(.arg1 ~ .arg2 +  .arg3 + .arg4)
fit$fitted
",
SUM([Profit]),
AVG([Sales]),
AVG([Quantity]),
AVG([Discount])
)
```

```
SCRIPT_REAL("
# R Code, variables replaced by placeholder
",
input variables
)
```

### Tableau and Python
Install `Tabpy` ```

