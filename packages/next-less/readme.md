Forked from [zeit/next-plugins](https://github.com/zeit/next-plugins).

# Next.js + Less

Import `.less` files in your Next.js project

## Installation

```
npm install --save @zeit/next-less less
```

or

```
yarn add @zeit/next-less less
```

## Usage

The stylesheet is compiled to `.next/static/css`. Next.js will automatically add the css file to the HTML. 
In production a chunk hash is added so that styles are updated when a new version of the stylesheet is deployed.

### Without CSS modules

Create a `next.config.js` in your project

```js
// next.config.js
const withLess = require('@zeit/next-less')
module.exports = withLess({
  /* config options here */
})
```

Create a Less file `styles.less`

```less
@font-size: 50px;
.example {
  font-size: @font-size;
}
```

Create a page file `pages/index.js`

```js
import "../styles.less"

export default () => <div className="example">Hello World!</div>
```

### With CSS modules

```js
// next.config.js
const withLess = require('@zeit/next-less')
module.exports = withLess({
  cssModules: true
})
```

Create a Less file `styles.less`

```less
@font-size: 50px;
.example {
  font-size: @font-size;
}
```

Create a page file `pages/index.js`

```js
import css from "../styles.less"

export default () => <div className={css.example}>Hello World!</div>
```

### With CSS modules and options

You can also pass a list of options to the `css-loader` by passing an object called `cssLoaderOptions`.

For instance, [to enable locally scoped CSS modules](https://github.com/css-modules/css-modules/blob/master/docs/local-scope.md#css-modules--local-scope), you can write:

```js
// next.config.js
const withLess = require('@zeit/next-less')
module.exports = withLess({
  cssModules: true,
  cssLoaderOptions: {
    importLoaders: 1,
    localIdentName: "[local]___[hash:base64:5]",
  }
})
```

Create a CSS file `styles.css`

```css
.example {
  font-size: 50px;
}
```

Create a page file `pages/index.js` that imports your stylesheet and uses the hashed class name from the stylesheet

```js
import css from "../style.css"

const Component = props => {
  return (
    <div className={css.backdrop}>
      ...
    </div>
  )
}

export default Component
```

Your exported HTML will then reflect locally scoped CSS class names.

For a list of supported options, [refer to the webpack `css-loader` README](https://github.com/webpack-contrib/css-loader#options).

### PostCSS plugins

Create a `next.config.js` in your project

```js
// next.config.js
const withLess = require('@zeit/next-less')
module.exports = withLess({
  /* config options here */
})
```

Create a `postcss.config.js`

```js
module.exports = {
  plugins: {
    // Illustrational
    'postcss-css-variables': {}
  }
}
```

Create a CSS file `styles.scss` the CSS here is using the css-variables postcss plugin.

```css
:root {
  --some-color: red;
}

.example {
  /* red */
  color: var(--some-color);
}
```

When `postcss.config.js` is not found `postcss-loader` will not be added and will not cause overhead.

You can also pass a list of options to the `postcss-loader` by passing an object called `postcssLoaderOptions`.

For example, to pass theme env variables to postcss-loader, you can write:

```js
// next.config.js
const withLess = require('@zeit/next-less')
module.exports = withLess({
  postcssLoaderOptions: {
    parser: true,
    config: {
      ctx: {
        theme: JSON.stringify(process.env.REACT_APP_THEME)
      }
    }
  }
})
```


### Configuring Next.js

Optionally you can add your custom Next.js configuration as parameter

```js
// next.config.js
const withLess = require('@zeit/next-less')
module.exports = withLess({
  webpack(config, options) {
    return config
  }
})
```
