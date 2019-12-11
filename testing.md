# Testing

{{ toc }}

## Running Tests

For the first time, complete all the following steps. On subsequent runs, only follow steps 2 and 4.

1. Create test database (if it doesn't exist yet) and fill it in with empty tables:

        mysql -u root -p -e "CREATE DATABASE {db_name}_test; GRANT ALL PRIVILEGES ON {db_name}_test.* TO '{db_user}'"
        php run migrations --fresh --env=testing

2. In separate session keep assets of the `testing` environment up-to-date:

        cd {project}
        npm run testing-watch 

3. Configure PHPUnit

        cd {project}
        php run config:phpunit -fmw 

4. Run The tests

        phpunit 

