# PHP CLI for Docker Wrapper for VSCode

This provides a PHP CLI wrapper for VS Code to run its language engine
on a remote ssh server when PHP is not available natively in the host
server.

## Installation

Until released, the following needs to be added to the project's `composer.json`:

```json
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/consilience/php-docker-cli.git"
        },
    ]
```

Then the packaged is installed using:

    composer require consilience/php-docker-cli --dev

From the host server, i.e. from outside the container running PHP, but where the
code is located, PHP commands can be run through docker at:

    vendor/bin/php-cli

For example:

```bash
$ vendor/bin/php-cli -v
PHP 7.4.13 (cli) (built: Dec 17 2020 09:02:29) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
```

When setting up the VSCode editor, the `php-cli` script is used as the PHP
executable. The PHP environment has access to the same filesystem as your
home directory.

## TODO

These are very early days, and this package is created to meet our specific use-case.
However, we would like to make it more flexible over time, and configurable.

The restrictions and assumptions that apply at the moment are:

* The latest alpine-7 PHP container is good enough and runs the version of PHP needed
  for the project.
* No special PHP extensions are required; this is just for VSCode to be able to validate
  and parse the project source code and run a PHP language server.
* All the source code is under `$HOME`. The `$HOME` path will be available as a volume
  within the container with the same root directory, so `/home/me/my-code` in the host
  server will also be `/home/me/my-code` within the PHP container.
* Docker is available and running.

Our use-case is VSCode connecting to a remote server with the source code located under
the user's home directory. The code was running in a Micro Kubernetes cluster, and no
PHP exectutable was available in the host server. VSCode must be able to run PHP on
the host server, so does so through the CLI command that this package provides
within the project.

By putting this into a package, it is quick and easy to require across multiple projects
without a lot of manual setup.

Further instructions on how and where to set up VSCode are on the TODO list.
