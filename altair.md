# import altair
```python
import altair as alt
```

# if in notebook
```python
alt.renderers.enable("notebook")
```

# sample histogram
```python
alt.Chart(df).mark_bar().encode(
    x = alt.X('Brain Weight', bin=alt.Bin(maxbins=20), title='Brain Weight'),
    y = "count()"
).properties(
    width=400,
    height=300,
    title="Histogram of Brain Weight"
)
```

# sample scatter plot
```python
alt.Chart(df).mark_circle().encode(
    x = "Body Weight",
    y = "Brain Weight"
).properties(
    width=850,
    height=300,
    title="Scatter Body & Brain Weights"
)
```

# sample bar chart
```python
alt.Chart(trends).mark_bar().encode(
    x = "search_term",
    y = "sum(value)",
    color = "search_term"
).properties(
    width=800,
    height=400,
    title="Google Trends by Search Term"
)
```

# bubble chart
```python
alt.Chart(lf).mark_circle().encode(
    x = "Country GDP",
    y = "Life Expectancy",
    size = "size",
    color = "Continent",
    tooltip = ["country", "Country GDP", "Life Expectancy"]
).properties(
    width=800,
    height=400
)
```

# map
```python
url="https://raw.githubusercontent.com/codeforamerica/click_that_hood/master/public/data/madrid.geojson"

madrid = alt.Data(url=url, format={"property":"features"})
alt.Chart(madrid).mark_geoshape().encode().properties(
    width=700,
    height=500
)
```

# horizontal concatenation
```python
trend_line | trend_bar
```

# vertical concatenation
```python
trend_line & trend_bar
```

# filtering charts
```python
select_search_trend = alt.selection_single(encodings=["x"])

trend_line = alt.Chart(trends).mark_line().encode(
    x = alt.X("date:T",timeUnit="yearmonth"),
    y = "sum(value)",
    color = "search_term"
).properties(
    width=600,
    height=400,
    title="Google Trends by Search Term"
).transform_filter(
    select_search_trend
)

trend_bar = alt.Chart(trends).mark_bar().encode(
    x = "search_term",
    y = "sum(value)",
    color = "search_term"
).properties(
    width=200,
    height=400,
    selection=select_search_trend
)

trend_line | trend_bar
```

# selecting intervals in charts
```python
select_interval_trend = alt.selection_interval(encodings=["x"])

trend_line = alt.Chart(trends).mark_line().encode(
    x = alt.X("date:T",timeUnit="yearmonth"),
    y = "sum(value)",
    color = "search_term"
).properties(
    width=600,
    height=400,
    title="Google Trends by Search Term",
    selection=select_interval_trend
)

trend_bar = alt.Chart(trends).mark_bar().encode(
    x = "search_term",
    y = "sum(value)",
    color = "search_term"
).properties(
    width=200,
    height=400
).transform_filter(
    select_interval_trend
)

trend_line | trend_bar
```

# zooming
```python
select_interval_trend = alt.selection_interval(encodings=["x"])

trends_line = alt.Chart(trends).mark_line().encode(
    x = alt.X("date:T",timeUnit="yearmonth"),
    y = "sum(value)",
    color = "search_term"
).properties(
    width=800,
    height=400,
    title="Google Trends by Search Term",
    selection=select_interval_trend
).transform_filter(
    select_interval_trend
)

trends_line_small = alt.Chart(trends).mark_line().encode(
    x = alt.X("date:T",timeUnit="yearmonth"),
    y = "sum(value)",
    color = "search_term"
).properties(
    width=800,
    height=100,
    selection=select_interval_trend
)

trends_line & trends_line_small
```

# dashboard example
```python
dashboard = (trend_line | trend_bar) & (trends_line & trends_line_small)
```
