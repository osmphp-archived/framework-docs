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
    
## Running Webpack In PHPStorm

You can run file-watching script in PHPStorm. 

### If Project Runs Locally

1. Open new terminal tab using `Alt+F12` keyboard shortcut.

2. Run the script:

        npm run watch
        
## If Project Runs On Remote Server

1. (first time only) Set up `Settings (Alt+F7) -> Tools -> SSH Terminal`:

    * Set `Deployment server` to the one you configured while creating the project.
    * Set `Default encoding` to `UTF-8`
    
2. Open new SSH terminal tab using `Ctrl+Shift+M H` keyboard shortcut.

3. Navigate to the project directory and run the script:

        cd {project_dir}
        npm run watch
                     