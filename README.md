# Giraffe

A React-based visualization library powering the data visualizations in [InfluxDB 2.0](https://github.com/influxdata/influxdb/) UI.

**This library is currently in pre-beta**

## 🦒 Features

There exist plenty of terrific visualization libraries in the JavaScript ecosystem.
Giraffe aims to distinguish itself with several features:

- Supports [Flux](https://www.influxdata.com/products/flux)
- Easy reactivity and extensibility via React
- Support for mapping groupings of columns to a single visual aesthetic
- A high-level [_Grammar of Graphics_](http://vita.had.co.nz/papers/layered-grammar.pdf) style API that can specify a wide variety of visualizations with a few simple concepts
- A [columnar](https://observablehq.com/@mbostock/manipulating-flat-arrays) interface for input data that enables efficient interop with Web Workers and [Apache Arrow](https://arrow.apache.org/)
- Self-contained configs in the style of [Vega-Lite](https://vega.github.io/vega-lite/)

## Demos

[See the visualizations in action using Storybook](https://influxdata.github.io/giraffe)

## Getting Started [](#getting-started)

#### Installation

Install [Giraffe](https://www.npmjs.com/package/@influxdata/giraffe) with your package manager

`yarn add @influxdata/giraffe` or `npm install @influxdata/giraffe`

#### Example

1. In your React code, import the `Plot` component and the `newTable` utility function

  <pre>
  import {Plot, newTable} from '@influxdata/giraffe'
  </pre>

2. Build the config object.  
   a. **Required properties**:

   - `table` is data built using the newTable utilty function (_also_ built from Flux results, _see_ [Flux example](./README.md#example-using-flux))
   - `layers` is an array of objects that describe how to render the data.

   b. _Optional properties_ include customizations for

   - gridlines: color and opacity
   - axes: appearance, color, opacity, and scaling
   - ticks: generation, formatting and labeling, font, and color
   - legend (tooltip): labeling and styling

   For details on all configuration properties, go to the [configuration guide](./giraffe/README.md#config).

   Here is an example of building the config object while skipping optional properties:

  <pre>
  // Example table and layer

  const table = newTable(5)
    .addColumn('_time', 'time', [1589838401244, 1589838461244, 1589838521244, 1589838581244, 1589838641244])
    .addColumn('_value', 'number', [2.58, 7.11, 4.79, 8.89, 2.23])

  const lineLayer = {
    type: "line",
    x: "_time",
    y: "_value",
  }

  const config = {
    table: table,
    layers: [lineLayer],
  }
  </pre>

3. Render your component by passing the `config` object as the config prop to the `<Plot>` component. **Be sure that the parent component around `<Plot>` has both a height and a width measured in positive values. If either is not a positive value, the graph will not be visible.**

   For example, to make a `<Plot>` that adjusts to screen height and width, in your React rendering code return this element:

  <pre>
  // return this element in your React rendering code:

  &#60;div
    style={{
      width: "calc(70vw - 20px)",
      height: "calc(70vh - 20px)",
      margin: "40px",
    }}
  &#62;
    &#60;Plot config={config} /&#62;
  &#60;/div&#62;
  </pre>

#### Example Using Flux [](#example-using-flux)

Use the string containing the comma separate values (CSV) from Flux as the `fluxResponse` property:

  <pre>
  import {Plot} from '@influxdata/giraffe'

  // ...

  const fluxResponse = `#datatype,string,long,dateTime:RFC3339,dateTime:RFC3339,dateTime:RFC3339,double,string,string,string,string
#group,false,false,true,true,false,false,true,true,true,true
#default,_result,,,,,,,,,
,result,table,_start,_stop,_time,_value,_field,_measurement,example,location
,,0,2020-03-25T20:58:15.731129Z,2020-04-24T20:58:15.731129Z,2020-04-03T18:31:33.95Z,29.9,value,temperature,index.html,browser
,,0,2020-03-25T20:58:15.731129Z,2020-04-24T20:58:15.731129Z,2020-04-03T18:55:23.863Z,28.7,value,temperature,index.html,browser
,,0,2020-03-25T20:58:15.731129Z,2020-04-24T20:58:15.731129Z,2020-04-03T19:50:52.357Z,15,value,temperature,index.html,browser
,,0,2020-03-25T20:58:15.731129Z,2020-04-24T20:58:15.731129Z,2020-04-03T19:53:37.198Z,24.8,value,temperature,index.html,browser
,,0,2020-03-25T20:58:15.731129Z,2020-04-24T20:58:15.731129Z,2020-04-03T19:53:53.033Z,23,value,temperature,index.html,browser
,,0,2020-03-25T20:58:15.731129Z,2020-04-24T20:58:15.731129Z,2020-04-03T20:19:21.88Z,20.1,value,temperature,index.html,browser
,,0,2020-03-25T20:58:15.731129Z,2020-04-24T20:58:15.731129Z,2020-04-10T22:20:40.776Z,28.7,value,temperature,index.html,browser
`

  const lineLayer = {
    type: "line",
    x: "_time",
    y: "_value",
  }

  const config = {
    fluxResponse,
    layers: [lineLayer],
  }

  // ...

  // return this element in your React rendering code:

  &#60;div
    style={{
      width: "calc(70vw - 20px)",
      height: "calc(70vh - 20px)",
      margin: "40px",
    }}
  &#62;
    &#60;Plot config={config} /&#62;
  &#60;/div&#62;
  </pre>

## Development

To contribute to Giraffe, see the [contributing guide](./CONTRIBUTING.md).

Looking for details on the configuration? See the [configuration guide](./giraffe/README.md#config).
