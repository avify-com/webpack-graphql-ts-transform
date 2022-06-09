# Webpack-graphql-ts-trasnform

This package was build to help servers using apollo-server and other technologies to compile graphql before going into your server to speed development time

## Before you start 
```bash
webpack-graphql-ts-transform
```
## Recommended packages for webpack with ts and graphql
This allows your server to have paths in tsconfig.json and resolve import package from '@package' style imports
```bash
npm i tsconfig-paths-webpack-plugin
```
```bash
npm i webpack-node-externals
```


## webpack.config.js file example
  ```javascript
   /* eslint-disable*/
const path = require('path');
const NODE_ENV = process.env.NODE_ENV || 'development';
const TsconfigPathsPlugin = require('tsconfig-paths-webpack-plugin');
const nodeExternals = require('webpack-node-externals');
const getTransformer =
  require('webpack-graphql-ts-transform').getTransformer;

module.exports = {
  entry: './src/index.ts',
  mode: NODE_ENV,
  optimization: {
    removeAvailableModules: false,
    removeEmptyChunks: false,
    splitChunks: false
  },
  module: {
    // Use `ts-loader` on any file that ends in '.ts'
    rules: [
      {
        test: /\.ts$/,
        use: [
          {
            loader: 'ts-loader',
            options: {
              configFile: 'tsconfig.json',
              getCustomTransformers: () => ({ before: [getTransformer()] })
            }
          }
        ],
        exclude: [
          /node_modules/,
          path.resolve(__dirname, './scripts'),
          path.resolve(__dirname, './src/test') //Remove this line if you don't have this dir
        ]
      }
    ]
  },
  // Bundle '.ts' files as well as '.js' files.
  resolve: {
    extensions: ['.ts', '.js', '.json', '.gql'],
    plugins: [new TsconfigPathsPlugin({})]
  },
  externals: [nodeExternals(), path.resolve(__dirname, './prisma')],
  output: {
    filename: 'index.js',
    path: `${process.cwd()}/build`,
    pathinfo: false
  },
  target: 'node16'
};

  ```

  ## How to support the package ?

  Create a pull request to help improve packages or any other issue you want us to resolve. :)