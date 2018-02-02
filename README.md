<div align="center">
  <img width="120" height="120" src="logo.svg">
  <h1>
    Speed Measure Plugin
    <div><em><sup><sub>(for webpack)</sub></sup></em></div>
  </h1>
</div>
<br>

The first step to optimising your webpack build speed, is to know where to focus your attention.

This plugin measures your webpack build speed, giving an output like this:

![Preview of Speed Measure Plugin's output](preview.png)

## Install

```bash
npm install --save speed-measure-webpack-plugin
```

or

```bash
yarn add speed-measure-webpack-plugin
```

## Usage

Change your webpack config from

```javascript
const webpackConfig = {
  plugins: [
    new MyPlugin(),
    new MyOtherPlugin()
  ]
}
```

to

```javascript
const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");

const webpackConfig = {
  plugins: SpeedMeasurePlugin.wrapPlugins({
    MyPlugin: new MyPlugin(),
    MyOtherPlugin: new MyOtherPlugin()
  })
}
```

If you're using `webpack-merge`, then you can do:

```javascript
const smp = new SpeedMeasurePlugin();

const baseConfig = {
  plugins: smp.wrapPlugins({
    MyPlugin: new MyPlugin()
  }).concat(smp)
  // ^ note the `.concat(smp)`
};

const envSpecificConfig = {
  plugins: smp.wrapPlugins({
    MyOtherPlugin: new MyOtherPlugin()
  })
  // ^ note no `.concat(smp)`
}

const finalWebpackConfig = webpackMerge([
  baseConfig,
  envSpecificConfig
]);

```

## Options

Options are passed in to the constructor

```javascript
const smp = new SpeedMeasurePlugin(options);
```

or as the second argument to the static `wrapPlugins`

```javascript
SpeedMeasurePlugin.wrapPlugins(pluginMap, options);
```

### `options.outputFormat`

Type: `String`<br>
Default: `"human"`

Determines in what format this plugin prints its measurements

 * `"json"` - produces a JSON blob
 * `"human"` - produces a human readable output

### `options.outputTarget`

Type: `String`<br>
Default: `undefined`

Specifies the path to a file to output to. If undefined, then output will print to `console.log`

### `options.disable`

Type: `Boolean`<br>
Default: `false`

If truthy, this plugin does nothing at all. It is recommended to set this with something similar to `{ disable: !process.env.MEASURE }` to allow opt-in measurements with a `MEASURE=true npm run build`
