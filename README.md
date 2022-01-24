# React setup with webpack & babel
- **Setup folder structure for react js** (you can create your own, but I am following default structure of react )
1.  create a new folder for webpack-react setup - (you can name your directory as you want)
2.  run `cd webpack-react`
3.  run `npm init -y` to create package.json with default configurations
4.  now create a `src` & `public` directory
5.  create `index.js` & `App.js` in `/src` & `index.html` in `/public`
- **That's it for react folder structure**
6.  now inside root folder run following commands to install dependency for react
`npm i react react-dom`
this will install react and reat-dom package into your project, that make you help to use react js.
7.  now write some code into `index.html`, `index.js` & `App.js`
index.html
```bash
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>React App</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```
index.js
```bash
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App'

ReactDOM.render(
    <App />,
  document.getElementById('root')
);
```
App.js
```bash
import React from 'react';

const App = () => {
  return <div>Hello react</div>;
};

export default App;

```
8.  now create `webpack.config.js` in your project root directory & install some more dependencies like babel, webpack-server & html-webpack plugin.
9.  Install webpack server
`npm install webpack webpack-cli webpack-dev-server --save-dev`
This command will install 3 packages `webpack`, `webpack-cli` **to run webpack commands** and `webpack-dev-server` to run react locally.
10. Now we have to setup webpack configurations. It is simply a file that will going to tell what dependencies have to run and when. Webpack needed following configuration:
-   **Entry** point for the app
-   **Loaders** what you want to load before showing the actual app
-   **Plugins** what plugins do you needed to run the app
-   **Output** where you want to store the output, mainly when you run build for production
-   **Mode** this are simple have to define in which mode we are running application `production` or `development`
11. Let's setup `Entry` point of the application and that is we want to set to index.js and it is in `webpack-react/src`.
```bash
const path = require('path');

module.exports = {
  entry: path.join(__dirname, "src", "index.js"),
}
```
Here we have to require path and you will get it by default from JS and not have to install any package.
12. Next, define loaders, but before that we have to install them in our project.
13. As we know we required babel to transpile our code in to javascript, here we are installing core babel compiler, babel-loader to load it, also, some presets that will tell babel what to transpile , install it by below command
`npm install @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev`
14. Set this in webpack config. After `entry`
```bash
module: {
    rules: [
      {
        test: /\.?js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ['@babel/preset-env', '@babel/preset-react']
          }
        }
      }]
```
15. We have setup this to process `.js` files, now it's obvious our application may have `.css`,`.png|jpeg|jpg|svg` files as well. Let's configure them as well.
16. To configure other files we do not have to install any package, just add following configuration in loaders, insider rules array.
```bash
{
    test: /\.css$/i,
    use: ["style-loader", "css-loader"],
},
{
    test: /\.(png|jp(e*)g|svg|gif)$/,
    use: ['file-loader'],
},
{
    test: /\.svg$/,
    use: ['@svgr/webpack'],
}
```
17. Now the next from the list is we have to setup plugins, so initially we needed `html-webpack-plugin` to tell webpack to inject whole bundle as a script tag to the HTML file.
`npm install html-webpack-plugin --save-dev`

18. Now configure this in webpack config, after loaders modules
```bash
plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "public", "index.html"),
    }),
  ]
```
19. Next, from the list setup **output** config in webpack config.
```bash
output: {
    path:path.resolve(__dirname, "dist"),
}
```
20. After that you have to do final config of webpack is the mode
```bash
mode: process.env.NODE_ENV === 'development' ? "developement" : "production",
```
So, the `webpack-config.js` now looks like:
```bash
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: path.join(__dirname, "src", "index.js"),
  output: {
    path:path.resolve(__dirname, "dist"),
  },
  module: {
    rules: [
      {
        test: /\.?js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ['@babel/preset-env', '@babel/preset-react']
          }
        }
      },
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.(png|jp(e*)g|svg|gif)$/,
        use: ['file-loader'],
      },
      {
        test: /\.svg$/,
        use: ['@svgr/webpack'],
      },
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "public", "index.html"),
    }),
  ],
  mode: process.env.NODE_ENV === 'development' ? "developement" : "production",
}
```
20. Next, we have to add some config in package.json to let the app know when I will run `npm start` what script it have to run.
```bash
"scripts": {
    "start": "webpack serve",
    "build": "NODE_ENV='production' webpack",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```
That's it.
Now, you can run app in development by using 
`npm start`
and create build
`npm run build`

Thanks!
