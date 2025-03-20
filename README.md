# Exercise: From Zero to Cry Baby -- Webpack to six pack --

<img width="598" alt="Screenshot 2568-03-20 at 19 17 05" src="https://github.com/user-attachments/assets/355860c3-08ec-492b-9184-41d6c479a1ee" />


**Objective:**
--------------

Understand the necessity of Webpack in modern web development by attempting to build a React project without it, then configuring Webpack to resolve the encountered issues.

**Step 1: Attempting to Build Without Webpack**
-----------------------------------------------

1.  Clone the project

```bash
git clone git@github.com:rmiyazaki6499/webpack-demo.git
```

2. Install dependencies (React, React router dom, http-server) *Please make sure you are using at least node v18*
        

```bash
yarn install
```

3. Run the project

```bash
yarn serve
```

You app should now run locally at http://localhost:8080

## Why it doesn't work

### Incorrect Script Source

**Issue**: Attempting to load `src/index.js` directly in `index.html`.

**Problem**: This file contains JSX, which browsers can't interpret without transpilation.

**Solution**: Webpack would transpile JSX into JavaScript, making it executable by browsers.

### No React Runtime

**Issue**: `index.js` imports React, but the library isn't loaded in the browser.

**Problem**: Without Webpack, you must manually include React in your HTML or ensure it's available.

**Solution**: Webpack would bundle React with your app, ensuring it's available when needed.

### No JSX Transformation

**Issue**: JSX syntax in your components needs to be transformed into JavaScript.

**Problem**: Without Webpack, manual transformation is required, which is impractical.

**Solution**: Webpack automates this transformation, allowing developers to write JSX without worrying about browser compatibility.

### No Module System

**Issue**: Use of ES6 module syntax (`import React from "react";`) in `index.js`.

**Problem**: Not all browsers support ES6 modules without transpilation or polyfills.

**Solution**: Webpack bundles all modules into a single file, handling module resolution and ensuring compatibility across browsers.

        

**Step 2: Configuring Webpack**
-------------------------------

1.  Install Webpack and Dependencies
    
    *   Install Webpack and necessary loaders:

```bash
yarn add -D webpack webpack-cli babel-loader @babel/core @babel/preset-react @babel/preset-env
```

2.  Create Babel Configuration
    
    *   Create a `.babelrc` file in the root directory with the following content:
        
```json
{
    "presets": ["@babel/preset-react", "@babel/preset-env"]
}
```

3.  Configure Webpack
    
    *   Create a `webpack.config.js` file in the root directory with the following content:
        
```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env', '@babel/preset-react'],
          },
        },
      },
    ],
  },
  resolve: {
    extensions: ['.js', '.jsx'],
  },
};
```

## Webpack Configuration Overview

### `module.exports = { ... }`

- **Purpose**: Exports the Webpack configuration object, making it available for use by Webpack.

#### `entry: './src/index.js'`

- **Purpose**: Specifies the entry point of your application. Webpack starts bundling from this file, creating a dependency graph.

#### `output: { ... }`

- **Purpose**: Defines where and how Webpack should output the bundled files.

  - **path**: Uses `path.resolve(__dirname, 'dist')` to set the output directory to `dist` in the current directory.
  - **filename**: Names the output bundle file as `bundle.js`.

#### `module: { ... }`

- **Purpose**: Configures how modules within the project should be treated.

  - **rules**: An array of rules that tell Webpack how to handle different file types.

    - **test**: A regular expression to match files with `.js` or `.jsx` extensions.
    - **exclude**: Excludes files in the `node_modules` directory from being processed by this rule.
    - **use**: Specifies the loader to use for processing matched files:
      - **loader**: Uses `babel-loader` to transpile JavaScript/JSX.
      - **options**: Configures Babel with presets for environment compatibility (`@babel/preset-env`) and React (`@babel/preset-react`).

#### `resolve: { ... }`

- **Purpose**: Configures how modules are resolved, allowing Webpack to find modules without specifying their full path or extension.

  - **extensions**: An array of file extensions that Webpack should consider when resolving modules. Here, it includes `.js` and `.jsx`, so you can import files without specifying the extension.

This configuration:

- Sets up a basic Webpack build for a React application.
- Uses Babel to transpile modern JavaScript and JSX into ES5 for browser compatibility.
- Specifies where the bundled code should be placed and how it should be named.
- Allows for easier module imports by automatically resolving file extensions.


4.  Modify index.html
    
    *   Update index.html to reference the bundled JavaScript file:

```xml
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>React App</title>
</head>
<body>
  <div id="root"></div>
  <script src="dist/bundle.js"></script>
</body>
</html>
```


5.  Build with Webpack

*   Run `yarn build`.

```bash
yarn build
```
    

1.  Serve the Project
    
    *   You can use a simple server like http-server to serve the index.html file from the root directory:
        
```bash
yarn serve
```

*   Run npm run serve or yarn serve, then open http://localhost:8080 in your browser.
    

**Conclusion:**
---------------

This exercise demonstrates how Webpack resolves issues related to module handling and JSX compilation, making it essential for modern web development with React.
