---
follows: followup1
---

## Visualization 5

{(aim|}
To understand how features that existed in every time period are distributed throughout those time periods
{|aim)}

{(vistype|}
Stacked and normalized stacked bar charts
{|vistype)}

```python
import altair as alt

places = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-places-latest.csv')

popularFeatureTypes = ['settlement', 'architecturalcomplex', 'fort', 'temple-2', 'bridge', 'mine', 'monument', 'region', 'tomb', 'sanctuary']

def stackedBarChart(column):
    return (
        alt.Chart(places).transform_calculate(
            groupedDate=f'datum.{column} < -550 ? "A" : datum.{column} < -330 ? "C" : datum.{column} < -30 ? "H" : datum.{column} < 300 ? "R" : "L"'
        ).mark_bar().encode(
            alt.X("groupedDate:O", sort=['A', 'C', 'H', 'R', 'L']),
            alt.Y('count()'),
            color='featureTypes:N',
            tooltip=['groupedDate:N', 'featureTypes:N', 'count()']
        ).transform_filter(
            alt.FieldOneOfPredicate(field='featureTypes', oneOf=popularFeatureTypes),
        ).interactive()
    )

alt.vconcat(
    alt.hconcat(
        stackedBarChart('minDate'),
        stackedBarChart('maxDate')
    ),
    alt.hconcat(
        stackedBarChart('minDate').encode(alt.Y('count()', stack='normalize')),
        stackedBarChart('maxDate').encode(alt.Y('count()', stack='normalize'))
    )
).properties(
    title='Stacked bar charts and corresponding normalized stacked bar charts showing popular features\' minDate and maxDate within each time period'
).configure_title(orient='top', anchor='middle')
```

![](https://raw.githubusercontent.com/David24E/CSCM37CW1/92d3d7db436152e42acdb6d490a4be97bb2a399e/finalVis.svg)

---

{(vismapping|}

Given that date is minDate or maxDate

x position
: date (grouped and sorted according to the group)

y position
: count of records

color
: featureTypes

tooltip
: date (grouped), featureTypes, count of each featureType

{|vismapping)}

{(dataprep|}
The data has been grouped to match the pleaides README on `timePeriods` such that the records fall into the following bins _"...'A' (1000-550 BC), 'C' (550-330 BC), 'H' (330-30 BC), 'R' (AD 30-300), 'L' (AD 300-640)"_. The data has also been filtered such that only a subset of `featureTypes` that are known to exist across all `timePeriods` are observed.
{|dataprep)}

{(limitations|}
This visualization could be improved by allowing users the ability to zoom into the chart. This visualization could be improved by randomly choosing what `featureTypes` to observe. Better still, improvements could be made by dynamically choosing what records to observe depending verious criteria, especially across the identified `timePeriods`. Further, input elements such as checkboxes, dropdowns and/or radio buttons could be used to allow dynamical selection of what criteria is used on the visualization.
{|limitations)}
