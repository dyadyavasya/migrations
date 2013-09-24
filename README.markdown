# Doctrine Database Migrations

## Features

* Single PHAR file for your projects
* `DiffCommand` for migrations (if an Entity Manager is available)
* Support for custom `ArgvInput` in CLI instance
* Support for custom `ConsoleOutput` in CLI instance

## Doctrine Migration Phar file

You can dowload latest version of [doctrine-migrations.phar](https://doc-0c-3c-docs.googleusercontent.com/docs/securesc/tiqi05gtu3t655ve90buujisbdgm0omo/90055finec8ko1b5q4cdcgv11kgliedo/1380024000000/15995912864489919224/15995912864489919224/0B6YZFgbTNhv6dno0RlZkZThTSjQ?e=download&h=16653014193614665626&nonce=0kk8p7cj08iuo&user=15995912864489919224&hash=v0cfj2lgn41rcv8e588p6lb6chs547vu). 
If you'll build your own `doctrine-migrations.phar` see instruction below.
 
## Installing Dependencies

You can install deps using [Composer] (http://getcomposer.org/):

    $   php composer.phar install

## Building the PHAR

    $   php package.php

It creates `./build/doctrine-migrations.phar`

### Creating archive disabled by INI setting

If you receive an error that looks like:

    creating archive "build/doctrine-migrations.phar" disabled by INI setting

This can be fixed by setting the following in your php.ini:

    ; http://php.net/phar.readonly
    phar.readonly = Off


## Configuration

### migrations.yml

Define how migrations will be stored and tracked within your database:

    name: Doctrine Sandbox Migrations
    migrations_namespace: DoctrineMigrations
    table_name: doctrine_migration_versions
    migrations_directory: /path/to/migrations/classes/DoctrineMigrations
    
By default Doctrine does not map the MySQL enum type to a Doctrine type. This is because Enums contain state (their allowed values) and Doctrine types don’t.
You can register MySQL ENUMs to map to Doctrine strings. This way Doctrine always resolves ENUMs to Doctrine strings.

    name: Doctrine Sandbox Migrations
    migrations_namespace: DoctrineMigrations
    table_name: doctrine_migration_versions
    migrations_directory: /path/to/migrations/classes/DoctrineMigrations
    mapping_types:
        enum: string

### migrations-db.php

Define how to connect to your database:
    
    return array(
        'driver'    => 'pdo_mysql',
        'host'      => 'localhost',
        'user'      => 'migrations',
        'password'  => 'm1gr@t10n$',
        'dbname'    => 'doctrine_sandbox'
    );

### migrations-input.php (Optional)

Specify defaults or provide your own custom `ArgvInput`, should you so desire:

    $input = new \Symfony\Component\Console\Input\ArgvInput;
    ... make some changes ...
    return $input;

### migrations-output.php (Optional)

If your database migrations contain HTML, you may run into issues with outputting the SQL to the console.
This is because the `ConsoleOutput` class uses HTML-like tags for styling certain messages, such as `error`s,
`info` messages, etc.

For HTML to render properly, you can customize the `ConsoleOutput` within this file as follows:

    $output = new \Symfony\Component\Console\Output\ConsoleOutput;
    $output->setStyle('p'); // Adds default styling to the 'p' tag, as to not throw a rendering exception

    return $output;

## Official Documentation

All available documentation can be found [here](http://docs.doctrine-project.org/projects/doctrine-migrations/en/latest/).
