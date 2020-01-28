# Installation

{{ toc }}

## Installing The Project

1. Create new project from template or clone existing project. It should also install NPM modules, run Webpack and configure file permissions. Run in shell:

        # for a new project
        composer create-project osmphp/osmproject {project}
        cd {project}
        
        # for an existing project
        git clone {project_repo_url} {project}
        cd {project}
        composer run-script post-root-package-install
        composer update

        # test that project's console commands work
        php run
        
2. (Optional) Create a database for the project.

        mysql -u root -p -e "CREATE DATABASE {db_name}; GRANT ALL PRIVILEGES ON {db_name}.* TO '{db_user}' IDENTIFIED BY '{db_password}'"
        php run env DB_NAME={db_name} 
        php run env DB_USER={db_user} 
        php run env DB_PASSWORD={db_password}
        php run migrations

3. (Optional) Install queue worker and other services, do other project-specific preparations.

## (Optional) Configuring The Web Server

1. If domain is not registered, add it to local `hosts` file. Run locally (if on Windows - with Administrator privileges):

        hosts add {server_ip} {project}

    **TODO**. Until `hosts` command is implemented, edit your system's `hosts` file manually.

2. Add the project to Web server configuration. Test HTTP. Run with `root` user:

        php run config:nginx

3. (Optional) Add SSL certificate and test HTTPS.

        # if domain is publicly registered
        certbot --nginx -d {project}

        # otherwise
        ssl create:cert {project}

## (Optional) Configuring The IDE

Configure your IDE. See also [PhpStorm: Opening A Project](tools/phpstorm.html#opening-a-project).

## Used Variables

* `project = {project}` - project directory name and Web domain name
* `project_repo_url = {project_repo_url}` - project Git repository URL
* `db_name = {db_name}` - MySql database name
* `db_user = {db_user}` - MySql user
* `db_password = {db_password}` - MySql user's password
* `server_ip = {server_ip}` - IP of the server on which the project is installed
