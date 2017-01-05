-This boilerplate illustrates isomorphic (universal) Webpack rendering using React.		 +This sample project illustrates isomorphic (universal) Webpack rendering using React.
 -Forked from https://github.com/halt-hammerzeit/webpack-react-redux-isomorphic-render-example

Features

* React
* React-router
* Redux
* Isomorphic (universal) rendering
* Webpack 2
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

Webpack 2
================

This project requires Webpack 2

```sh
npm install webpack@2.2.0-rc.3
```

I've been using Webpack 1 for a long time here while Webpack 2 was in beta. Now the time has come to move to Webpack 2 since it's [almost ready to be released](https://github.com/webpack/webpack/milestone/10).

See [Webpack 1 to 2 migration notes](https://webpack.js.org/guides/migrating/)

See older versions of this repo for Webpack 1 compatibility.
