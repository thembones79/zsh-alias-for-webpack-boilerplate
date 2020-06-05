# ZSH Alias for Webpack Boilerplate



Just add this code to your ~/.zshrc


```


webpack_boilerplate() {
    mkdir $1 &&
    cd $1 &&
    npm init -y &&
    touch .gitignore &&
    echo node_modules >> .gitignore &&
    echo dist >> .gitignore
    rm -rf package.json &&
    touch package.json &&
    echo -n '{
    "name": "'>> package.json &&
    echo -n $1 >> package.json &&
    echo '",
    "version": "1.0.0",
    "private": true,
    "description": "",
    "main": "index.js",
    "scripts": {
      "start": "webpack-dev-server --config webpack.dev.js --open",
      "build": "webpack --config webpack.prod.js"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"
    }'>> package.json &&
    touch webpack.common.js &&
    echo 'module.exports = {
      entry: { main: "./src/index.js", vendor: "./src/vendor.js" },
      module: {
        rules: [
          {
            test: /\.html$/,
            use: ["html-loader"],
          },
          {
            test: /\.(svg|png|jpg|gif)$/,
            use: {
              loader: "file-loader",
              options: {
                name: "[name].[hash].[ext]",
                outputPath: "imgs",
              },
            },
          },
        ],
      },
    };
    ' >> webpack.common.js &&
    touch webpack.dev.js &&
    echo 'const path = require("path");
    const common = require("./webpack.common");
    const merge = require("webpack-merge");
    const HtmlWebpackPlugin = require("html-webpack-plugin");

    module.exports = merge(common, {
      mode: "development",
      output: {
        filename: "[name].bundle.js",
        path: path.resolve(__dirname, "dist"),
      },
      plugins: [
        new HtmlWebpackPlugin({
          template: "./src/index.html",
        }),
      ],
      module: {
        rules: [
          {
            test: /\.scss$/,
            use: ["style-loader", "css-loader", "sass-loader"],
          },
        ],
      },
    });' >> webpack.dev.js &&
    touch webpack.prod.js &&
    echo 'const path = require("path");
    const common = require("./webpack.common");
    const merge = require("webpack-merge");
    const { CleanWebpackPlugin } = require("clean-webpack-plugin");
    const MiniCssExtraxtPlugin = require("mini-css-extract-plugin");
    const OptimizeCssAssetsPlugin = require("optimize-css-assets-webpack-plugin");
    const TerserPlugin = require("terser-webpack-plugin");
    const HtmlWebpackPlugin = require("html-webpack-plugin");

    module.exports = merge(common, {
      mode: "production",
      output: {
        filename: "[name].[contentHash].js",
        path: path.resolve(__dirname, "dist"),
      },
      optimization: {
        minimizer: [
          new OptimizeCssAssetsPlugin(),
          new TerserPlugin(),
          new HtmlWebpackPlugin({
            template: "./src/index.html",
            minify: {
              removeAttributeQuotes: true,
              collapseWhitespace: true,
              removeComments: true,
            },
          }),
        ],
      },
      plugins: [
        new MiniCssExtraxtPlugin({ filename: "[name].[contentHash].css" }),
        new CleanWebpackPlugin(),
      ],
      module: {
        rules: [
          {
            test: /\.scss$/,
            use: [MiniCssExtraxtPlugin.loader, "css-loader", "sass-loader"],
          },
        ],
      },
    });
    ' >> webpack.prod.js &&
    mkdir src &&
    cd src &&
    touch index.js &&
    touch vendor.js &&
    echo '// here you can import some vendors libs
    //import "bootstrap";' >> vendor.js &&
    touch index.html &&
    echo '<!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
      </head>
      <body></body>
    </html>' >> index.html &&
    cd .. &&
    npm i --save-dev -E webpack webpack-cli webpack-dev-server clean-webpack-plugin webpack-merge node-sass css-loader file-loader style-loader sass-loader html-loader html-webpack-plugin mini-css-extract-plugin optimize-css-assets-webpack-plugin
}



```


## Usage:

```

webpack_boilerplate your-project-name

```

This will setup a vanilla JS frontent project for you with dev and prod envs and SASS support.

Typescript & React versions still to come.

Enjoy 😀

