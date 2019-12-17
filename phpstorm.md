# PhpStorm

{{ toc }}

## Opening A Project

In case the project is remote, create/clone it locally the same way you did it on server, see also step 1 in [Installing The Project](installation.html#installing-the-project). Then configure it in PHPStorm:

1. ["Exclude" directories](https://www.jetbrains.com/help/phpstorm/configuring-folders-within-a-content-root.html) containing generated files, in `Settings (Alt+F7) -> Directories`:

        public/[development|testing|web|backend|frontend]
        temp/[development|testing|production]

2. [Configure deployment](https://www.jetbrains.com/help/phpstorm/configuring-synchronization-with-a-remote-host.html) - connection, automatic upload and exclusions, in `Settings (Alt+F7) -> Build, Execution, Deployment -> Deployment`:

        node_modules
        public/[development|testing|web|backend|frontend]
        temp/[development|testing|production]

3. Add Git repositories of library packages to [Version Control](https://www.jetbrains.com/help/phpstorm/settings-version-control.html) configuration in `Settings (Alt+F7) -> Version Control`.

## JS Import Statements

Out of the box, PHPStorm doesn't understand JS import statements mentioning Osm modules, like the one below:

    import broadcasts from 'Osm_Framework_Js/vars/broadcasts';

Configure PHPStorm to understand such declarations: 

1. Run in shell, in project's directory:

        php run config:phpstorm 
        
2. Set `Settings (Alt+F7) -> Languages & Frameworks -> JavaScript -> WebPack -> webpack configuration file = {project}/temp/development/webpack.phpstorm.config.js`.      

3. If needed, restart PHPStorm.  