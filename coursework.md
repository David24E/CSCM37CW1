---
narrative-schemas:
  - coursework
---

## Visualization 1

{(aim|}
To get an overview of the data by understanding the distribution of the recorded coordinates, any identified points of interest and their relative location on the world map as we know it today.
{|aim)}

{(vistype|}
Map
{|vistype)}

```python
import altair as alt
from vega_datasets import data

places = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-places-latest.csv')
graticule = alt.topo_feature(data.world_110m.url, 'countries')

featureTypesSelector = alt.selection_single(empty='all', fields=['featureTypes'])

mapBase = alt.Chart(graticule).mark_geoshape(
    fill='lightgray',
    stroke='white'
).project(
    'equirectangular'
).properties(
    width=750,
    height=500
)

placePoints = alt.Chart(places).mark_circle().encode(
    latitude='reprLat:Q',
    longitude='reprLong:Q',
    tooltip=['title:N', 'featureTypes:N']
).transform_filter(
    featureTypesSelector
).properties(
    title="Identified coordinates for the pleiades places dataset"
).add_selection(featureTypesSelector)

(mapBase + placePoints).configure_view(stroke=None)
```

![](courseworkVis.svg)

---

{(vismapping|}

latitude
: reprLat

longitude
: reprLong

tooltip
: title, featureTypes

{|vismapping)}

{(dataprep|}
The data is dynamically filtered such that when a data point on the map is clicked, only data points that are of the same featureType as the point clicked are rendered on the map. Clicking on any space on the map besides a data point resets the filter.
{|dataprep)}

{(limitations|}
More interaction with the map such as the ability to pan and zoom would be beneficial. Further, being able to show the connections between places might help provide more context and, thus, a better understanding of the dataset.
{|limitations)}
