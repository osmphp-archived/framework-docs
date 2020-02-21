# Migrations

A **migration** is a PHP class which creates new tables, modifies existing tables or fills the tables with initial data.

Migrations are module-specific. For example, module responsible for blog posts would have a migration creating blog post table, module responsible for post comments would have a migration creating a table for storing comments and so on.

A module may have more than one migration. For example, `App_Posts` module may have a migration which creates a table for storing blog posts. As table structure emerges you may add additional migrations to alter the blog post table. 

Contents:

{{ toc }}

## Running Migrations 

Run migrations in the project's directory:

    php run migrations
    
First, it runs **schema migrations** for each module, that is, creates/alters table structure. Then it runs **data migrations** for each module - fills the tables with data. Modules are migrated in order specified by their dependencies.

Migrations files within a single module are numbered (`01-posts.php`, `02-post_timestamps.php` and so on) and executed in the specified order.

Osm framework tracks which migrations are installed. Running `php run migrations` command once again will skip migrations which had already ran earlier and only run new ones.

New migrations are ran automatically after `composer update`, so if modules in the installed Composer packages receive an update containing new migrations they will automatically apply to the database.

During development you may need to recreate the database anew. You can so that using `--fresh` parameter:

    php run migrations --fresh
    
After you uninstall a module, you can roll back its migrations too:

    php run migrations-back App_Posts
    
## Creating Migrations

To create a schema migration, [make your module current](../core/modules.html#current-module) and then run in the project's directory:

    osm create:migration posts
    
To create a data migration, use `--step` option:

    osm create:migration posts --step=data
    
Either way, the command creates a migration class in the module's `migrations` directory:

    class Posts extends Migration
    {
        public function up() {
    
        }
    
        public function down() {
        }
    }

`up()` is executed by `php run migrations` command and `down()` is executed by `php run migrations-back` command.

## `up()` Method

In `up()` method, use one of these APIs for creating tables and filling them with initial data:

1. [Laravel Schema and Query Builder](laravel-schema-and-query-builder.html)

    * Most closely maps to the underlying raw SQL queries
    * Well documented and widely used

2. [Tables](tables.html) and [Formula Queries](formula-queries.html)

    * Built on top of (1)  
    * Allows unlimited number of table columns
    * Allows custom column settings which can be used in project's logic
    * Better support for PHPStorm code completion
    * Compact query syntax
    * Automated table relations 
    * In future, the same query API will work over in-memory arrays

3. [Sheets](sheets.html) and [Sheet Operations](sheet-operations.html)

    * Built of top of (2)
    * Facet counting
    * Full-text search
    * Virtual (calculated) columns
    * Editable columns with default calculated values
    * Batch editing of multiple records
    * The data source of the [Data Table](../ui-library/data-tables.html) UI view

**Notice**. Whichever API you use for creating a table, use the same API for working with its data. For instance, if you created a table using Laravel API, don't try to use formula queries over it.  

## `down()` Method 

`down()` method may be omitted except if:

* `up()` method creates a new table - in this case `down()` method should drop it
* `up()` method alters other module's table - in this case `down()` method should undo those changes.

## Stability

Once a migration is released (and used in production), don't change, rename or delete it. If you need to change the table structure, create a new migration for that. 

Also keep an eye on the code you use in the migration scripts. 

If, let's say, you use a some custom method which creates a table with a single `id` column in your migration and after some time the same method is changed to create `id` and `created_at` columns instead, then it is essentially the same as modifying a migration file.

If such changes happen, create a new migration file which would check tables created with the mentioned method and add `created_at` column to them.

To avoid this king of problems, stick to using one 3 APIs listed above. These APIs are stable and even if they change in future there will be a clear procedure on how to migrate previously created tables to the updated structure.    

## Migration Naming Conventions

If you rename a migration while in development, do rename both the migration file and the migration class. If for example new migration name is `02_some_table.php`, the class name should be `SomeTable`. 
 