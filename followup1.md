---
follows: coursework
---

## Visualization 2

{(aim|}
To understand the correlation between the latitude and longitude coordinates across each pleiades dataset and the precision of the coordinates
{|aim)}

{(vistype|}
Dot Dash Plot with linked histogram
{|vistype)}

```python
import altair as alt

locations = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-locations-latest.csv')
names = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-names-latest.csv')
places = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-places-latest.csv')

brush = alt.selection_interval()
tick_axis = alt.Axis(labels=False, domain=False, ticks=False)

def scatter(source):
    return (
        alt.Chart(source).mark_circle(opacity=0.75).encode(
            x='reprLat:Q',
            y='reprLong:Q',
            color=alt.condition(brush, 'locationPrecision:N', alt.value('lightgray')),
            tooltip=['title:N', 'reprLatLong:N', 'minDate:N', 'maxDate:N']
        ).add_selection(brush)
    )

def xTicks(source):
    return alt.Chart(source).mark_tick().encode(
        alt.X('reprLat:Q', axis=tick_axis),
        alt.Y('locationPrecision:N', title='', axis=tick_axis),
        color=alt.condition(brush, 'locationPrecision:N', alt.value('lightgrey')),
    ).add_selection(brush)

def yTicks(source):
    return alt.Chart(source).mark_tick().encode(
        alt.X('locationPrecision:N', title='', axis=tick_axis),
        alt.Y('reprLong:Q', axis=tick_axis),
        color=alt.condition(brush, 'locationPrecision:N', alt.value('lightgrey'))
    ).add_selection(brush)

def histo(source):
    return (
        alt.Chart(source).mark_bar().encode(
            x='locationPrecision:N',
            y='count(locationPrecision):Q',
            color='locationPrecision:N',
            tooltip=['count(locationPrecision):N']
        ).transform_filter(
            brush
        ).interactive()
    )

def dotDashPlot(sources):
    return yTicks(sources) | (scatter(sources) & xTicks(sources)) | histo(sources)

alt.vconcat(
    (dotDashPlot(locations) | dotDashPlot(names)),
    dotDashPlot(places)
).properties(
    title='Dot dash plots and linked histograms comparing the correlation between reprLat, reprLong, a measure of their accuracy for the records and their distribution on each pleiades dataset'
).configure_title(orient='top', anchor='middle').configure_view(stroke=None)
```

![](oneVis.svg)

---

{(vismapping|}

For each pleiades dataset,

scatter plot
: **x position:** reprLat
: **y position:** reprLong
: **color:** conditionally locationPrecision or lightgray
: **tooltip:** title, reprLatLong, minDate, maxDate

---

x-axis strip plot
: **x position:** reprLat
: **y position:** locationPrecision
: **color:** conditionally locationPrecision or lightgray

---

y-axis strip plot
: **x position:** locationPrecision
: **y position:** reprLong
: **color:** conditionally locationPrecision or lightgray

---

histogram
: **x position:** locationPrecision
: **y position:** count of locationPrecision records
: **color:** locationPrecision
: **tooltip:** count of locationPrecision records

{|vismapping)}

{(dataprep|}
Linked brushing is set up such that when an area on a scatter plot is highlighted, corresponding areas on the other scatter plots are highlighted and the connected strip plot and histogram for each are dynamically updated. Clicking on any space on any scatter plot besides a data point resets the visualization.
{|dataprep)}

{(limitations|}
The visualization could be improved by adding input elements such as checkboxes, dropdowns and/or radio buttons to allow the ability to choose what coordinates to display based on a selected precision. A line of best fit might also be beneficial in understanding the correlation betwen the latitude and longitude data channels.
{|limitations)}
