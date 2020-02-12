# Data File Watcher

**Data file watcher** is a [Gulp](https://gulpjs.com/) script which detects new, modified, renamed and deleted files in the project's `data` directory and invokes PHP code to handle those changes.

Use data file watcher to clear clear cache entries derived from the data files or to update index structures using the data files.

Contents:

{{ toc }}

## Installing Gulp

To install Gulp globally, run with `root` user:

        npm install --global gulp-cli

## Running Data File Watcher

### Using Supervisor

Use [Supervisor](http://supervisord.org/) to run the data file watcher script as a background service (daemon).

1. To install Supervisor, run with `root` user:

        apt-get install supervisor
        
2. Create `/etc/supervisor/conf.d/{project}__watch_data.conf` file:

        [program:{project}__watch_data]
        process_name=%(program_name)s_%(process_num)02d
        directory={home}/{project}
        command=npm run watch-data
        autostart=true
        autorestart=true
        user=root
        numprocs=1
        redirect_stderr=true
        stdout_logfile=/var/log/supervisor/%(program_name)s.log        

3. Start the data file watcher service with `root` user:

        supervisorctl reread
        supervisorctl update
        supervisorctl start {project}__watch_data:*

Later, stop, start or restart the data file watcher service with `root` user:

    supervisorctl stop {project}__watch_data:*
    supervisorctl start {project}__watch_data:*
    supervisorctl restart {project}__watch_data:*

### In Shell

Alternatively, run the data file watcher script in the project directory:

    npm run watch-data
    
## Handling Data File Changes

1. Add a dependency in your module class:

        class Module extends BaseModule
        {
            public $hard_dependencies = [
                'Osm_Framework_Gulp',
            ];
        }

2. Add your data file watcher class in your module's `config/file_watchers.php` file:

        <?php
        
        use {module_namespace}\FileWatcher;
        
        return [
            'books' => FileWatcher::class,
        ]; 
        
3. Write data file watching logic in the `handle()` method of your data file watcher class:

        use Osm\Framework\Gulp\FileWatcher as BaseFileWatcher;

        class FileWatcher extends BaseFileWatcher
        {
            public function handle($paths) {
                // write the logic here
            }
        }
        
    The `$paths` parameter is an array of absolute paths of all new, modified, renamed or deleted files.
    
## Watching Additional Directories

You can customize the list of watched directories using a [dynamic trait](../core/dynamic-traits.html):

1. Register a dynamic trait in your module class:    

        use Osm\Framework\Gulp\Commands\ConfigGulp;

        class Module extends BaseModule
        {
            public $traits = [
                ConfigGulp::class => Traits\ConfigGulpTrait::class,
            ];
        }

2. Customize the list of watched directories in the dynamic trait:

        namespace {module_namespace}\Traits;
        
        trait ConfigGulpTrait
        {
            protected function around_getWatchedPatterns(callable $proceed) {
                // prepare an array of additional file patterns to watch
                $patterns = [ '/home/vagrant/my-data/**/*.*' ];
        
                return array_merge($proceed(), $patterns);
            }
        }

3. Restart the data file watcher service with `root` user:

        supervisorctl restart {project}__watch_data:*
