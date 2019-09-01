# With Webpack

Start by installing the Webpack plugin as a dev dependency:

```text
npm i -D purgecss-webpack-plugin
```

You will need `extract-text-webpack-plugin` as well.

```text
npm i -D extract-text-webpack-plugin
```

```javascript
const path = require('path')
const glob = require('glob')
const ExtractTextPlugin = require('extract-text-webpack-plugin')
const PurgecssPlugin = require('purgecss-webpack-plugin')

const PATHS = {
  src: path.join(__dirname, 'src')
}

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: 'style-loader',
          use: 'css-loader?sourceMap'
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin('[name].css?[hash]'),
    new PurgecssPlugin({
      paths: glob.sync(`${PATHS.src}/*`)
    })
  ]
}
```

## Options

The options available in the PurgeCSS [configuration](https://www.purgecss.com/configuration.html) are also available in the Webpack plugin \(except `css` and `content`\).

* paths

With the Webpack plugin, you can specify content that should be analyzed by PurgeCSS with an array of filenames. The files can be HTML, Pug, Blade, etc. You can also use a module like `glob` or `glob-all` to easily get a list of files.

```javascript
const PurgecssPlugin = require('purgecss-webpack-plugin')
const glob = require('glob')
const PATHS = {
  src: path.join(__dirname, 'src')
}

// In the webpack configuration
new PurgecssPlugin({
  paths: glob.sync(`${PATHS.src}/*`)
})
```

If you want to regenerate the paths list on every compilation \(e.g. with `--watch`\), then you can also pass a function:

```javascript
new PurgecssPlugin({
  paths: () => glob.sync(`${PATHS.src}/*`)
})
```

* only

You can specify entrypoints to the purgecss-webpack-plugin with the option `only`:

```javascript
new PurgecssPlugin({
  paths: glob.sync(`${PATHS.src}/*`),
  only: ['bundle', 'vendor']
})
```

* **whitelist and whitelistPatterns**

Similar as for the `paths` option, you also can define functions for the these options:

```javascript
function collectWhitelist() {
    // do something to collect the whitelist
    return ['whitelisted'];
}
function collectWhitelistPatterns() {
    // do something to collect the whitelist
    return [/^whitelisted-/];
}

// In the Webpack configuration
new PurgecssPlugin({
  whitelist: collectWhitelist,
  whitelistPatterns: collectWhitelistPatterns
})
```

