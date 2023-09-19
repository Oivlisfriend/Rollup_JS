

# Rollup_JS
<div style="display: inline_block">
<img alt="npm" src="https://img.shields.io/badge/npm-CB3837?style=for-the-badge&logo=npm&logoColor=white">
<img alt="npm" src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB">
</div><br>
Rollup is a module bundler for JavaScript which compiles small pieces of code into something larger and more complex, such as a library or application.
<br><br>

    mkdir expense-manager-rollup
    cd expense-manager-rollup
Create and initialize the project

    npm init -y 
Install React libraries (react and react-dom)

    npm install react@^17.0.0 react-dom@^17.0.0 --save 
Install babel and its preset libraries as development dependency.
       
    npm install @babel/preset-env @babel/preset-react @babel/core @babel/pluginproposal-class-properties -D
Install rollup and its plugin libraries as development dependency

    npm i -D rollup postcss@8.1 @rollup/plugin-babel @rollup/plugin-commonjs 
    @rollup/plugin-node-resolve @rollup/plugin-replace rollup-plugin-livereload 
    rollup-plugin-postcss rollup-plugin-serve postcss@8.1 postcss-modules@4 rollupplugin-postcss
Install corejs and regenerator runtime for async programming

    npm i regenerator-runtime core-js
    
Create a babel configuration file, .babelrc under the root folder to configure the babel 
compiler

      {
        "presets": [
            [
                "@babel/preset-env",
                {
                    "useBuiltIns": "usage",
                    "corejs": 3,
                    "targets": "> 0.25%, not dead"
                }
            ],
            "@babel/preset-react"
        ],
        "plugins": [
            "@babel/plugin-proposal-class-properties"
        ]
    }
Create a rollup.config.js file in the root folder to configure the rollup bundler

    import babel from '@rollup/plugin-babel';
    import resolve from '@rollup/plugin-node-resolve';
    import commonjs from '@rollup/plugin-commonjs';
    import replace from '@rollup/plugin-replace';
    import serve from 'rollup-plugin-serve';
    import livereload from 'rollup-plugin-livereload';
    import postcss from 'rollup-plugin-postcss'
    export default {
      input: 'src/index.js',
      output: {
        file: 'public/index.js',
        format: 'iife',
      },
      plugins: [
        commonjs({
          include: [
            'node_modules/**',
          ],
          exclude: [
            'node_modules/process-es6/**',
          ],
        }),
        resolve(),
        babel({
          exclude: 'node_modules/**',
          babelHelpers: 'bundled',
        }),
        replace({
          'process.env.NODE_ENV': JSON.stringify('production'),
          preventAssignment: true, 
        }),
        postcss({
          autoModules: true
        }),
        livereload('public'),
        serve({
          contentBase: 'public',
          port: 3000,
          open: true,
        }), 
      ]
    }
Update the package.json and include our entry point (public/index.js and  public/styles.css) and command to build and run the application:

    "main": "public/index.js",
    "style": "public/styles.css",
    "files": [
      "public"
    ],
    "scripts": {
      "start": "rollup -c -w",
      "build": "rollup"
    },

- Create a src folder in the root directory of the application, which will hold all the 
source code of the application.
- Create a folder, components under src to include our React components. The idea is to create two files, <component>.js to write the component logic and <component.css>
to include the component specific styles. The final structure of the application will be as follows
-Let us create a new component, HelloWorld to confirm our setup is working fine. Create a file, HelloWorld.js under components folder and write a simple component to emit **Hello World** message:
#
    import React from "react";
    class HelloWorld extends React.Component {
     render() {
       return (
         <div>
           <h1>Hello World!</h1>
         </div>
       );
     }
    }
    export default HelloWorld;
Create our main file, index.js under src folder and call our newly created component:
#
    import React from 'react';
    import ReactDOM from 'react-dom';
    import HelloWorld from './components/HelloWorld';
    ReactDOM.render(
     <React.StrictMode>
     <HelloWorld />
     </React.StrictMode>,
    document.getElementById('root')
    );
- Create a public folder in the root directory.
Create a html file, index.html (under public folder*), which will be our entry point of the application.
#
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8">
        <title>Expense Manager :: Rollup version</title>
      </head>
      <body>
      <div id="root"></div>
       <script type="text/JavaScript" src="./index.js"></script>
      </body>
    </html>
Finally, build and run the application:

    npm start
The npm build command will execute the rollup and bundle our application into a single file, dist/index.js file and start serving the application. The dev command will recompile the code whenever the source code is changed and also reload the changes in the browser
#
    rollup v3.29.2
    bundles src/index.js → public/index.js...
    LiveReload enabled
    http://localhost:3000 ->/path/to/your/workspace/expense-manager-rollup/dist
    created public/index.js in 15.5s

    [2023-09-19 06:02:56] waiting for changes...
Next, open the browser and enter http://localhost:3000 in the address bar and press enter. serve application will serve our webpage as shown below.
#
    Hello World
