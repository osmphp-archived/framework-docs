# `dev` Area

`dev` area will show all the configuration and will allow to generate new files and generate into existing files:

* It is only for local development.

* With Vagrant, you'll have to use synced folder as files will be generated on Vagrant machine and they have to be somehow transferred to the host machine. Use `777` permissions for directories and `666` for files.

* Web pages should be able to run processes. Don't forbid that in `php.ini`.

Still, it is nice to have. CLI is good enough for developers.