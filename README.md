---
title: CSCM37 Assignment 1 --- Information visualization
author: David Edeh
Visualization Tool Used: Altair(Python)
---

# Setup

There are 3 data files from [Pleiades](https://pleiades.stoa.org/) each having records for places, names, and locations in the database. Generally speaking, all visualizations in this repository depend on and begin with the following code block in order to import altair and access the relevant pleiades datasets.

```python
import altair as alt

locations = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-locations-latest.csv')
names = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-names-latest.csv')
places = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-places-latest.csv')
```

---

Each visual design has been reported in the style of the required layout with the following literate visualization labels:

Aim (`aim`):
: Things we can learn from the visualization, e.g., from this visualization
we can see this pattern...

Visual Design Type (`vistype`):
: The name/type of the visual design

Image:
: The visualization itself as an image

Visual Mappings (`vismapping`):
: Each of the visual design mappings. Include the data mapping
information about color, shape, size, position (x,y axes), and any other
visual mappings.

Data Preparation (`dataprep`):
: Any modifications to the original data that had to be performed to
generate your beautiful image.

Improvements (`improvements`):
: What are the limitations of your visualization? What improvements could
you make to it?

---

The final submission has been laid out as:

```
921295
|- README.md
|- coursework.md
|- followup1.md
|- followup2.md
|- followup3.md
|- final.md
|- 921295.pdf
|- coursework.yml
|- courseworkVis.svg
|- oneVis.svg
|- twoVis.svg
|- threeVis.svg
|- final.svg
|- ...
```
