This boilerplate illustrates isomorphic (universal) Webpack rendering using React.
Forked from https://github.com/halt-hammerzeit/webpack-react-redux-isomorphic-render-example

Features

* React
* React-router
* Redux
* Isomorphic (universal) rendering
* Webpack
* Development mode: hot reload for React components, hot reload for Redux reducers

Quick Start
===========

* `npm install`
* `npm run dev`
* wait for it to finish (it will say "Now go to http://127.0.0.1:3000" in the end)
* go to `http://localhost:3000`
* interact with the development version of the web application
* `Ctrl + C`
* `npm run production`
* wait a bit for Webpack to finish the build (green stats will appear in the terminal, plus some `node.js` server running commands)
* go to `http://localhost:3000`
* interact with the production version of the web application

Webpack 2 (beta)
================

This project installs Webpack v1 by default. And it also works with the latest beta of Webpack 2 (`25` at the time of writing).

The only change required is to remove `postcss` from the configuration and add `LoaderOptionsPlugin` instead:

```js
configuration.plugins.push
(
	new webpack.LoaderOptionsPlugin
	({
		test: /\.scss$/,
		options:
		{
			// A temporary workaround for `scss-loader`
			// https://github.com/jtangelder/sass-loader/issues/298
			output:
			{
				path: configuration.output.path
			},

			postcss: [autoprefixer({ browsers: 'last 2 version' })],

			// A temporary workaround for `css-loader`.
			// Can also supply `query.context` parameter.
			context: configuration.context
		}
	})
)
```

`LoaderOptionsPlugin` seems to have additional options that might be configured possibly by adding a second instance of the same plugin:

```js
// For production mode
// https://moduscreate.com/webpack-2-tree-shaking-configuration/
new webpack.LoaderOptionsPlugin
({
	minimize: true,
	debug: false
})
```
