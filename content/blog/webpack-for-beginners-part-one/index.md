---
title: Webpack for beginners (Part 1)
description: Let's start with what webpack.js.org tells us about the core concepts.
date: "2019-08-09T13:46:37.121Z"
---

**Estimated Read Time: 10 mins**

Before you start reading this post, let's check if you if you fall under the following category: 

* You are convinced of the reasons for using Webpack as your project's module bundler.

* You know how to install webpack as a dev dependency in your project. 

Now, let's get into it.

# What is Webpack?

Let's start with what [webpack.js.org](https://webpack.js.org/concepts/) tells us about the core concepts.

> At its core, webpack is a static module bundler for modern JavaScript applications. When webpack processes your application, it internally builds a dependency graph which maps every module your project needs and generates one or more bundles.

That was pretty straightforward. So essentially, Webpack takes **ALL** your source code, together with your **installed packages** (React, Redux, Lodash, etc), and creates one or many bundles (depending on your config) for both **DEV** and **PROD** environments. 

For the purpose of explaining Webpack, we will refer to React Boilerplate (link found [here](https://github.com/react-boilerplate/react-boilerplate). Taking reference from a Boilerplate just makes life easier to begin with. You can always choose to start from scratch or edit the webpack config files once you have a good understanding. 

# Show me some code!

**Webpack version**: v4.30.0

Here's the entry point for running the webpack service.

Refer to **package.json** for the configuration options. We will break them down one by one in the following section.

```json
"build": "cross-env NODE_ENV=production webpack --config internals/webpack/webpack.prod.babel.js --color -p --progress --hide-modules --display-optimization-bailout",
```

The first thing it does is to set the environment to **production** seen in **NODE_ENV=production**.

Next, it runs wepback and passes along several options such as 

```json{numberLines: false}
internals/webpack/webpack.prod.babel.js
```

By default, webpack looks for the config file in the main directory. In this example, the webpack.prod.babel.js file is found in a custom directory, therefore it needs to be passed as an option to webpack.

```json
--color
```

This displays the console output in color.

```json
-p
```

This options is a shortcut for --optimize-minimize --define process.env.NODE_ENV="production".

```json
--progress
```

This means to print onto console the progress percentage of the build/bunding process.

```json
--hide_modules
```

This hides information about modules.


```json
--display-optimzation-bailout
```

This enables debugging messages for cases for modules that cannot be optimized by merging into a single scope. If you wish to read more, you can find out more about [ModuleConcatenationPlugin](https://webpack.js.org/plugins/module-concatenation-plugin/).


# The configuration files

The wepback configurations are split into 3 files. 

* webpack.base.babel.js 
* webpack.dev.babel.js
* webpack.prod.babel.js

The base file ontains the common configuration to both **development** and **production** environments. 

Now, if you're new to Webpack, you will most likely try to get the most basic version of the config working. And gradually, code bloat will crep up on you. One day, you will notice that the config file has become a monster with plenty of code duplication. That's the point where you will want to de-dup your code and extract the common config to a common file.

```javascript
/**
 * COMMON WEBPACK CONFIGURATION
 */

const path = require('path');
const webpack = require('webpack');

module.exports = options => ({
  mode: options.mode,
  entry: options.entry,
  output: Object.assign(
    {
      // Compile into js/build.js
      path: path.resolve(process.cwd(), 'build'),
      publicPath: '/',
    },
    options.output,
  ), // Merge with env dependent settings
  optimization: options.optimization,
```

The above code block takes in a few options and sets it to the respective setting type. **options** is a basic javaScript object that contains **mode**, **entry**, and **output** that is passed in from **webpack.dev.babel.js** or **webpack.prod.babel.js** files. 

**Mode** is typically **development** or **production**.

**Entry** tells Webpack where the main index file of your project resides. This is the same as the main React.js index.js.

**Output** instructs Webpack where to store the generated/compiled javascript bundle / bundles.

**Optimization** accepts the optimizations settings from the parent file, which will be explained in detail in Part 2.

# Loaders vs Plugins

Although these two concepts are central to Webpack, they are not exactly intuitive. In my personal experience, I read about it once or twice and forget about what they mean. It's probably until the 3rd or 4th time that I finally register it permanently. 

At a high level, let's take a look at what these two concepts actually mean.

## Loaders
Loaders work on the individual file level during or before the bundle is generated.

## Plugins
Plugins work at the bundle or chunk level and usually work at the end of the bundle compilation process. They can modify how the bundles themselves are created. It's the backbone of Webpack, and is really powerful.

Let's dive into the **Loaders** and **Plugins** code below.

**webpack.base.babel.js**
```javascript
module: {
    rules: [
      {
        test: /\.jsx?$/, // Transform all .js and .jsx files required somewhere with Babel
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: options.babelQuery,
        },
      },
```

The **module** object contains **Loaders** that you intend to use. And **rules** is basically an array that accepts objects that includes **test** (regex expression to read the file(s) that the loader should work on), **exclude** (tells Webpack which folders/files you wish to exclude), and the name of the loader and additional options are passed to the **use** object.


```javascript
  plugins: options.plugins.concat([
    // Always expose NODE_ENV to webpack, in order to use `process.env.NODE_ENV`
    // inside your code for any environment checks; Terser will automatically
    // drop any unreachable code.
    new webpack.EnvironmentPlugin({
      NODE_ENV: 'development',
    }),
  ]),
  resolve: {
    modules: ['node_modules', 'app'],
    extensions: ['.js', '.jsx', '.react.js'],
    mainFields: ['browser', 'jsnext:main', 'main'],
  },
  devtool: options.devtool,
  target: 'web', // Make web variables accessible to webpack, e.g. window
  performance: options.performance || {},
```

The first thing you probably noticed is the **plugins** array. Just like **rules** above, you can pass into **plugins** an array of various **plugins**. You would probably also noticed **options.plugins.concat**, which basically takes the plugins passed in from either **dev** or **prod** file and adds the **webpack.EnvironmentPlugin** into the existing array of plugins.

We see here that **resolve** accepts **modules**, **extensions** and **mainFields**.

**Modules** basically tells Webpack which directory to search when attempting to resolve module. Here, the absolute paths of **node_modules** and **app** is passed in.

**Extensions** tells Webpack the order by which it attempts to resolve import dependencies in the application.

```javascript
import File from '../path/to/file';
```
That allows you to left out the file type while importing files. Cool huh?

# In Closing

Phew... Ok I think that's pretty much enough for today. 

The Webpack core team has actually made Webpack a lot easier to configure since version 1 by simplying the configuration required and the documentation is comprehensive and pretty beginner-friendly. So if you feel up for it, take a look at the official webpack documentation [here](https://webpack.js.org/concepts/) and figure things out on your own.

Stay tuned for Part 2 where I will explain about both the **webpac.dev.babel.js** and **webpack.prod.babel.js** fles.



