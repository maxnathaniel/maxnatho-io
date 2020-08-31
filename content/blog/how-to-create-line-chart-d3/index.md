---
title: How to create a Basic Line Chart in D3.js
description: Tutorial on creating a Basic Line Chart in D3.js
date: "2020-08-31T23:46:37.121Z"
---

# What is D3.js?
> D3.js is a JavaScript library for manipulating documents based on data. D3 helps you bring data to life using HTML, SVG, and CSS.

That's about it. I would add that it's the most powerful and customizable JavaScript charting library out there right now. You can do virtually anything you want with it.

[D3.js](https://https://d3js.org/)

But if you are here, I assume you already know what D3, so let's dive straight into the code.

We first load the data using the `csv` method. It returns a promise so we grab the data from within the `then()` method.

```javascript
d3.csv("/data.csv", d3.autoType)
```
Next, we transform the raw data to our own format.

```javascript
const data = Object.assign(d.map(v => ({ date: v.date, value: v.value} )), { y: "$ Close" });
```

If you are using ES6, you can destructure it like this -

```javascript
const data = Object.assign(d.map(({ date, close }) => ({ date, value: close } )), { y: "$ Close" });
```

We set some defaults and proceed to create the main `line` charting object. Fairly straightforward stuff here.

```javascript
const height = 400;

const width = 1200;

const line = d3.line()
  .defined(d => !isNaN(d.value))
  .x(d => x(d.date))
  .y(d => y(d.value));

const margin = {
  top: 20,
  right: 30,
  bottom: 30,
  left: 40
};
```
Now, we create the x and y axis.

```javascript
const x = d3.scaleUtc()
  .domain(d3.extent(data, d => d.date))
  .range([margin.left, width - margin.right]);

const y = d3.scaleLinear()
  .domain([0, d3.max(data, d => d.value)]).nice()
  .range([height - margin.bottom, margin.top]);

const xAxis = g => g
  .attr("transform", `translate(0, ${ height - margin.bottom })`)
  .call(d3.axisBottom(x)
    .ticks(width / 80)
    .tickSizeOuter(0));

const yAxis = g => g
  .attr("transform", `translate(${margin.left}, 0)`)
  .call(d3.axisLeft(y))
  .call(g => g.select(".domain").remove())
  .call(g => g.select(".tick:last-of-type text").clone()
    .attr("x", 3)
    .attr("text-anchor", "start")
    .attr("font-weight", "bold")
    .text(data.y));
```

We will cover some of the interesting pieces in the code above.

* `d3.extent` returns the `min` and `max` value of the array.

* `.call(g => g.select(".tick:last-of-type text").clone()` basically clones the last item in the y-axis.

The rest of the code basically appends the various elements onto the `DOM`.

```javascript
const svg = d3.select("svg")
  .attr("viewBox", [0, 0, width, height]);

svg.append("g")
  .call(xAxis);

svg.append("g")
  .call(yAxis);

svg.append("path")
  .datum(data)
  .attr("fill", "none")
  .attr("stroke", "steelblue")
  .attr("stroke-width", 1.5)
  .attr("stroke-linejoin", "round")
  .attr("stroke-linecap", "round")
  .attr("d", line);
});
```

There we have it!

![Line Chart](./d3-line-chart.png)


