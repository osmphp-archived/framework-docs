# Laravel Schema And Query Builder

Internally, Osm framework uses Laravel schema builder and query builder in database-related code. You can do, too.

Contents:

{{ toc }}

## Using Schema Builder 

1. Use `schema` property in your migration file:

        $this->schema->create('posts', function(Blueprint $table) {
            $table->increments('id');

            $table->string('name');
            $table->unique('name');
        });


TODO 