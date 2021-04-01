---
follows: coursework
---

## Visualization 3

{(aim|}
To understand the correlation between the minDate and maxDate for the records across each pleiades dataset
{|aim)}

{(vistype|}
Binned bubble plot and histogram
{|vistype)}

```python
import altair as alt

locations = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-locations-latest.csv')
names = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-names-latest.csv')
places = ('https://raw.githubusercontent.com/SwanseaU-TTW/csc337_coursework1/master/pleiades-places-latest.csv')

def binnedBubblePlot(title, source):
    return (
        alt.Chart(source).transform_calculate(
            groupedMinDate='datum.minDate < -550 ? "A" : datum.minDate < -330 ? "C" : datum.minDate < -30 ? "H" : datum.minDate < 300 ? "R" : "L"'
        ).transform_calculate(
            groupedMaxDate='datum.maxDate < -550 ? "A" : datum.maxDate < -330 ? "C" : datum.maxDate < -30 ? "H" : datum.maxDate < 300 ? "R" : "L"'
        ).mark_point().encode(
            alt.X("groupedMinDate:O", sort=['A', 'C', 'H', 'R', 'L']),
            alt.Y("groupedMaxDate:O", sort=['A', 'C', 'H', 'R', 'L']),
            size='count():N',
            tooltip=['count()']
        ).properties(width=250, height=250, title=f'{title}')
    )

def histo(source, column):
    return (
        alt.Chart(source).transform_calculate(
            groupedDate=f'datum.{column} < -550 ? "A" : datum.{column} < -330 ? "C" : datum.{column} < -30 ? "H" : datum.{column} < 300 ? "R" : "L"'
        ).mark_bar().encode(
            alt.X("groupedDate:O", sort=['A', 'C', 'H', 'R', 'L'], title=f'{column}(grouped)'),
            alt.Y('count()'),
            color='groupedDate:N',
            tooltip=['groupedDate:N', 'count()']
        ).interactive()
    )

def combi(title, source):
    return (
        binnedBubblePlot(title, source) & (histo(source, 'minDate') | histo(source, 'maxDate'))
    )

alt.hconcat(
    combi('locations', locations), combi('names', names), combi('places', places)
).properties(
    title='Binned Bubble plots and corresponding histograms comparing the correlation between the minDate and maxDate for the records on each pleiades dataset'
).configure_title(orient='top', anchor='middle')
```

![](https://raw.githubusercontent.com/David24E/CSCM37CW1/92d3d7db436152e42acdb6d490a4be97bb2a399e/twoVis.svg)

---

{(vismapping|}

For each pleiades dataset,

binned bubble plot
: **x position:** minDate (grouped and sorted according to the group)
: **y position:** maxDate (grouped and sorted according to the group)
: **tooltip:** count of records

---

Given that date is minDate or maxDate

histogram
: **x position:** date (grouped and sorted according to the group)
: **y position:** count of records
: **color:** date (grouped and sorted according to the group)
: **tooltip:** count of records

{|vismapping)}

{(dataprep|}
For each dataset, the data has been grouped to match the pleaides README on `timePeriods` such that the records fall into the following bins _"...'A' (1000-550 BC), 'C' (550-330 BC), 'H' (330-30 BC), 'R' (AD 30-300), 'L' (AD 300-640)"_.
{|dataprep)}

{(limitations|}
The size scale could be more carefully chosen for better visibility of all bubbles, especially those on the smaller end of the spectrum. Then, for easier, quicker comparisons between each chart, each bubble could be overlay with the count of records it represent as opposed to relying for tooltips. The charts could also be organised and sized such that for each bubble plot, the corresponding `minDate` histogram is below it and the `maxDate` histogram is 'turned' on its left side and places to the left of the bubble plot and the scales line up.
{|limitations)}
