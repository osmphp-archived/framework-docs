# Migrations

A **migration** is a PHP class which creates new tables, modifies existing tables or fills the tables with initial data.

Migrations are module-specific. For example, module responsible for blog posts would have a migration creating blog post table, module responsible for post comments would have a migration creating a table for storing comments and so on.

A module may have more than one migration. For example, `App_Posts` module may have a migration which creates a table for storing blog posts. As table structure emerges you may add additional migrations to alter the blog post table. 

Contents:

{{ toc }}

## Determinism

The main use case for migrations is introducing database schema changes in a live production system in an deterministic way. A migration should make the same changes as your modules evolve or new modules are installed.

**Example**. A migration add a `INT` column to some table using some class' `createIntColumn()` method. After 

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

## Renaming Migrations

## `up()` Method

## `down()` Method 

`down()` method may be omitted except if:

* `up()` method creates a new table - in this case `down()` method should drop it
* `up()` method alters other module's table - in this case `down()` method should undo those changes.