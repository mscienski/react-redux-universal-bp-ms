This sample project is a proof-of-concept for isomorphic (universal) Webpack rendering using React.

Features

* React
* React-router
* Redux as Flux
* Isomorphic (universal) rendering
* Webpack
* Koa
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

Motivation
==========

In summer 2015 I wrote [`webpack-isomorphic-tools`](https://github.com/halt-hammerzeit/webpack-isomorphic-tools) to make isomorphic (universal) React rendering work on server-side when the project was built with Webpack.

The goal was met and many people started using it to implement isomorphic (universal) rendering in their apps.

Half a year passed and then I decided to give a try to the obscure enough [`target: 'node'`](http://stackoverflow.com/questions/26063480/how-to-simultaneously-create-both-web-and-node-versions-of-a-bundle-with-web) Webpack option which is the officially recommended way of setting up isomorphic (universal) rendering when building apps with Webpack. I used [sokra](https://github.com/sokra)'s [`react-starter`](https://github.com/webpack/react-starter) as a reference with a goal of making it much easier to use and much more developer-friendly.

Status
======

Seems to work.

If you have any issues running this code you can report them the issue tracker.

Brief explanation
=================

It runs two Webpack compilers in parallel: one for client and one for server.

To be more specific, in production mode it runs `webpack --config "webpack/production build"` which returns an array of two configurations: one for client and one for webpage rendering server (generated by `react-isomorphic-render/webpack` library).

The webpage rendering server Webpack configuration [main differences](https://github.com/halt-hammerzeit/react-isomorphic-render/blob/master/source/webpack/build%20server.js) are:

 * `target: "node"` instead of `target: "web"` which compiles code for Node.js usage instead of a web browser usage
 * `style-loader` is removed from the module loader chain for `css` files. Webpack author says that `css-loader` has to be changed into `css-loader/locals`: i didn't do that because it broke my styles but those using "css modules" feature might try to do that.

In development mode it runs two processes in parallel: the first one is `node "webpack/development"` which creates a client-side Webpack configuration and runs an instance of Webpack with this configuration along with `webpack-dev-server` (so it doesn't output any assets onto the disk because they are served by `webpack-dev-server` from memory); the second one is `webpack --config "webpack/build server dev"` which builds the webpage rendering server.

Client side build configuration also has `react-isomorphic-render/webpack` plugin enabled which emits a file called `webpack-assets.json` containing the URLs for the `main.[hash].js` and `main.[hash].css` (and for other entry points if any). This file is then read by webpage rendering server to generate proper Html webpage code (containing proper `<script src="main.js"/>` and `<link rel="stylesheet" href="main.css"/>` tags).

Client code is compiled into `build/assets/` with the `main.[hash].js` entry point (compiled from `code/client/application.js`). This is the `main` "chunk" and usually it is the complete bundle in a sense that it includes all of the `require()`d `node_modules` inside itself. That's why this thing is called "web-pack": because it packs all your code along with all the dependencies needed into a (usually) single javascript file.

Webpage rendering server code is compiled into `build/assets/webpage_rendering_server.js` entry point (compiled from `code/page-server/web server.js`). This is also a "chunk". In this case it doesn't contain all of the `node_modules` needed just to reduce the build time. You can always change this behaviour by replacing `configuration.externals` of the `configuration` returned by the `server(base_configuration, options)` function call.

Each React page component has a `preload` method which is executed before the page is rendered.

To do
==========

 * Maybe fix the "Clean webpack did not delete path: ...\webpack-react-redux-isomorphic-render-example\build\assets" error

 * Maybe make server side rendering Webpack code source maps work for exception stack traces. Currently Node.js [doesn't support source maps](https://github.com/nodejs/node-v0.x-archive/issues/3712).

 * Maybe try multiple entry points on client side (nah)