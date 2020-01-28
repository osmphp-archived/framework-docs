# Modules

Osm application is made of **modules**. Each module handles relatively simple task with well-defined boundaries. 

For example, typical blog application would have a module for rendering blog posts in frontend and editing them in backend, a module for handling user registration and login, a module for commenting on blog posts, a module for searching the blog and maybe more.

Most modules you'll create will be specific to your project. Application-specific modules are covered in this article. 

However, you can also create modules in a Composer package and share them among several projects. Actually, Osm framework brings lots of standard modules with its `osmphp/framework` Composer package. Reusable modules are covered in [Packages](packages.html).  

Contents:

{{ toc }}

## Module-Related Commands

### Listing Installed Modules

To see a list of currently installed modules, run in the project's directory:

    osm modules

### Creating A Module

To create a new module, run in the project's directory:

    osm create:module Posts

This commands creates the module directory `app/src/Posts`. 

Module full name (or just **module name**) is `App_Posts`, you can see it by running `osm modules` command. 

Base namespace of module's PHP classes is `App\Posts`. 

`osm create:module` command also makes new module the **current module**. It means that further  `osm create:*` commands will generate files in the current module directory, that is, `app/src/Posts`. 

## Module Class

At the very least, the module directory contains one PHP class `App\Posts\Module` also known as the **module class**.

In runtime, the module class is used to create a singleton module object. You can access the module object in runtime by module name:

    global $osm_app;
    $module = $osm_app->modules['App_Posts'];  


### Module Dependencies

Module load order is important and you can specify which modules should be loaded before this module - **module dependencies**. Module dependencies are either: 

* **hard dependency** - if the dependency is not installed, the module will fail to load
* **soft dependency** - it is OK is dependency is missing 

Specify module dependencies in the properties of the module class:

    class Module extends BaseModule
    {
        public $hard_dependencies = [
            'Osm_Framework_Migrations',
            'Osm_Framework_Js',
        ];
    
        public $soft_dependencies = [
            'My_Json',
        ];
    }

Despite specified dependencies, `App_*` modules are loaded after the modules defined in Composer packages, and in particular, after all the Osm framework modules. 

### Dynamic Traits

Using dynamic traits, you can customize public or protected methods of almost any class of other module.  

Example:

    use Osm\Core\Properties;

    class Module extends BaseModule
    {
        public $traits = [
            Properties::class => Traits\PropertiesTrait::class,
        ];
    }

See also [Dynamic Traits](dynamic-traits.html).

### Module-Specific Global Variables

Use custom module properties as module-specific global variables of the application.

Defining a property:

    /**
     * @property QueryLoggingClasses|string[] $query_logging_classes @required
     */
    class Module extends BaseModule
    {
        protected function default($property) {
            global $osm_app; /* @var App $osm_app */
    
            switch ($property) {
                case 'query_logging_classes': 
                    return $osm_app->cache->remember('query_logging_classes', function($data) {
                        return QueryLoggingClasses::new($data);
                    });
            }
            return parent::default($property);
        }
    }

Using the property:

    global $osm_app;
    $module = $osm_app->modules['App_Posts']; 
    $classes = $module->query_logging_classes; 

### Bootstrapping Code

Define what happens when the application is initialized and terminated in the methods of the module class:

    public function boot() {
    }

    public function terminate() {
    }

## Module Directory Structure

### Special Directories

Some directories inside the module directory have special meaning:

* `config/` - settings which the module contributes into the application configuration. See also [Configuration](configuration.html).
* `migrations/` - migrations scripts which should be executed in order to install module-specific tables and data to the database (and to uninstall back if needed). See also [Migrations](../databases/migrations.html).
* `resources/`, `frontend/`, `backend/` and other area asset directories contain module-specific [Blade templates](../web-pages/templates.html), [layers](../web-pages/layers.html), [JS](../scripts.html), [CSS and other files](../styles.html).   

### PHP Classes

All the other directories and files contain PHP classes. 
