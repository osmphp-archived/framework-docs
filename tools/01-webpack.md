# Webpack

Short version: while making changes to source files, run Webpack file watching script in project's directory:

    npm run watch

Restart it if needed.

Long version:  
 
{{ toc }}

## Running Webpack

Run Webpack in project's directory:

    npm run webpack

This command:

* deleted cache files;
* bundles `.scss` and `.js` files to `public` directory;
* copies image and font files to `public` directory;
* copies layer and template files to `temp` directory.

## Watching Files For Changes

Run Webpack file watcher in project's directory:

    npm run watch
    
This command watches all source files for changes and runs Webpack on modified files.

Restart this command after: 

* adding/removing modules or themes;

* adding/removing Webpack **entry point files** in existing module area asset directory:

        js/index.js
        critical-js/index.js
        css/styles.scss

* adding/removing Webpack **entry point directories** in existing module area asset directory:
        
        layers/
        views/

## Using Webpack In `testing` Environment

For `testing` environment, Webpack and file-watching commands are:

    npm run testing-webpack
    npm run testing-watch