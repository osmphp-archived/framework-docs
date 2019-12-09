# PhpStorm

{{ toc }}

## Opening A Project

In case the project is remote, create/clone it locally the same way you did it on server,see also step 1 in [Installing The Project](installation.html#installing-the-project). Then configure it in PHPStorm:

1. ["Exclude" directories](https://www.jetbrains.com/help/phpstorm/configuring-folders-within-a-content-root.html) containing generated files:

        public/[development|testing|web|backend|frontend]
        temp/[development|testing|production]

2. [Configure deployment](https://www.jetbrains.com/help/phpstorm/configuring-synchronization-with-a-remote-host.html) - connection, automatic upload and exclusions:

        .env
        node_modules
        public/[development|testing|web|backend|frontend]
        temp/[development|testing|production]

3. Add Git repositories of library packages to [Version Control](https://www.jetbrains.com/help/phpstorm/settings-version-control.html) configuration.
