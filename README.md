[![Build Status](https://travis-ci.org/leroy-merlin-br/mongolid-laravel.svg?branch=master)](https://travis-ci.org/leroy-merlin-br/mongolid-laravel)
[![Coverage Status](https://coveralls.io/repos/github/leroy-merlin-br/mongolid-laravel/badge.svg?branch=master)](https://coveralls.io/github/leroy-merlin-br/mongolid-laravel?branch=master)
[![Latest Stable Version](https://poser.pugx.org/leroy-merlin-br/mongolid-laravel/v/stable.png)](https://packagist.org/packages/leroy-merlin-br/mongolid-laravel)
[![Monthly Downloads](https://poser.pugx.org/leroy-merlin-br/mongolid-laravel/d/monthly.png)](https://packagist.org/packages/leroy-merlin-br/mongolid-laravel)
[![Latest Unstable Version](https://poser.pugx.org/leroy-merlin-br/mongolid-laravel/v/unstable.png)](https://packagist.org/packages/leroy-merlin-br/mongolid-laravel)
[![License](https://poser.pugx.org/leroy-merlin-br/mongolid-laravel/license.png)](https://packagist.org/packages/leroy-merlin-br/mongolid-laravel)

![MongoLid](https://user-images.githubusercontent.com/1991286/28967747-fe5c258a-78f2-11e7-91c7-8850ffb32004.png)

# MongoLid (Laravel Package)

- [Introduction](#introduction)
- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Authentication](#authentication)
- [Troubleshooting](#troubleshooting)
- [License](#license)
- [Additional Information](#additional_information)

## Introduction

MongoLid ODM (Object Document Mapper) provides a beautiful, simple implementation for working with MongoDB. Each database collection can have a corresponding "Model" which is used to interact with that collection.

> Note: The ODM implementation is within the [(non laravel) mongolid repository](https://github.com/leroy-merlin-br/mongolid).

## Installation

Install with `composer require` (use one of the above tags if needed).

```shell
composer require leroy-merlin-br/mongolid-laravel
```

**Note:** Mongolid Laravel 2.0 only supports Laravel 5.4+. For older versions use the tags:

- Laravel 4.2 `"leroy-merlin-br/mongolid-laravel": "^0.7"`
- Laravel 5.1 `"leroy-merlin-br/mongolid-laravel": "2.0.0-beta4"`
- Laravel 5.2 `"leroy-merlin-br/mongolid-laravel": "2.0.0-beta6"`
- Laravel 5.3 `"leroy-merlin-br/mongolid-laravel": "2.0.0-beta6"`


Make sure to set minimum stability to `dev` when using a beta tag (`composer config minimum-stability dev`).

> **Note**: If you are using Laravel 5.5, the next steps for providers and aliases are unnecessaries. MongoLid supports Laravel new [Package Discovery](https://laravel.com/docs/5.5/packages#package-discovery).

In your `config/app.php` add `'MongolidLaravel\MongolidServiceProvider'` to the end of the `$providers` array

```php
'providers' => [
    Illuminate\Translation\TranslationServiceProvider::class,
    Illuminate\Validation\ValidationServiceProvider::class,
    Illuminate\View\ViewServiceProvider::class,
    ...
    MongolidLaravel\MongolidServiceProvider::class,
],
```

(**Optional**) At the end of `config/app.php` add `'MongoLid'    => 'MongolidLaravel\MongoLidModel'` to the `$aliases` array

```php
'aliases' => [
    'App'         => Illuminate\Support\Facades\App::class,
    'Artisan'     => Illuminate\Support\Facades\Artisan::class,
    ...
    'MongoLid'    => MongolidLaravel\MongoLidModel::class,
],
```

Lastly, be sure to configure a database connection in `config/database.php`:

Paste the settings bellow at the end of your `config/database.php`, before the last `];`:

**Notice:** It must be **outside** of `connections` array.

```php
/*
|--------------------------------------------------------------------------
| MongoDB Databases
|--------------------------------------------------------------------------
|
| MongoDB is a document database with the scalability and flexibility
| that you want with the querying and indexing that you need.
| Mongolid Laravel use this config to starting querying right now.
|
*/

'mongodb' => [
    'default' => [
        'host'     => env('DB_HOST', '127.0.0.1'),
        'port'     => env('DB_PORT_NUMBER', 27017),
        'database' => env('DB_DATABASE', 'my_database'),
        'username' => env('DB_USERNAME', null),
        'password' => env('DB_PASSWORD', null),
    ],
],
```

For cluster with automatic failover, you need to set `cluster` key containing all `nodes` along with `replica_set` name.

```php
'mongodb' => [
    'default' => [
        'cluster' => [
            'replica_set' => env('DB_REPLICA_SET', null),
            'nodes' => [
                'primary' => [
                    'host' => env('DB_HOST_A', 'host-a'),
                    'port' => env('DB_PORT_A', 27017),
                ],
                'secondary' => [
                    'host' => env('DB_HOST_B', 'host-b'),
                    'port' => env('DB_PORT_B', 27017),
                ],
            ],
        ],
        'database' => env('DB_DATABASE', 'mongolid'),
        'username' => env('DB_USERNAME', null),
        'password' => env('DB_PASSWORD', null),
    ],
],
```

You can configure as much nodes as needed, node names (e.g. `primary` and `secondary` ) are optional.

> **Note:** If you don't specify the `mongodb` key in your `config/database.php` MongoLid will automatically try to connect to '127.0.0.1:27017' and use a database named 'mongolid'.

You may optionally provide a `connection_string` key to set a fully-assembled connection string that will override all other connection options. More info about connection string are found in [MongoDB documentation](https://docs.mongodb.com/manual/reference/connection-string/).

```php
'mongodb' => [
    'default' => [
        'connection_string' => 'mongodb://host-a:27017,host-b:27917/mongolid?replicaSet=rs-ds123',
    ],
],
```

Also, it is possible to pass `options` and `driver_options` to MongoDB Client. Mongolid always overrides `typeMap` configuration of `driver_options` to `array` because it makes easier to use internally with models. Possible `options` and `driver_options` are present on [`MongoDB\Client` documentation](https://docs.mongodb.com/php-library/master/reference/method/MongoDBClient__construct/).

## Basic Usage

To get started, create an MongoLid model. Models typically live in the `app/models` directory, but you are free to place them anywhere that can be auto-loaded according to your `composer.json` file.

**Defining a MongoLid Model**

```php
<?php
namespace App;

use MongolidLaravel\MongolidModel;

class User extends MongolidModel
{

}
```

In a nutshell, that's it!

### For further reading about models, CRUD operations, relationships and more, check the [Read the Docs: <small>leroy-merlin-br.github.com/mongolid</small>](http://leroy-merlin-br.github.io/mongolid/).
[![Mongolid Docs](https://user-images.githubusercontent.com/1991286/28967747-fe5c258a-78f2-11e7-91c7-8850ffb32004.png)](http://leroy-merlin-br.github.com/mongolid)

## Authentication

MongoLid Laravel comes with a Laravel auth provider.
In order to use it, simply change the `'driver'` provider value in your `config/auth.php` to `mongolid`
and make sure that the class specified in `model` is a MongoLid model that implements the `Authenticatable` contract:

```php

    'providers' => [

        // ...

        'users' => [
            'driver' => 'mongolid',
            'model' => \App\User::class
        ],

        // ...

    ],

```

The `User` model should implement the `Authenticatable` interface:

```php
<?php
namespace App;

use Illuminate\Contracts\Auth\Authenticatable;
use MongolidLaravel\MongolidModel;

class User extends MongolidModel implements Authenticatable
{
    /**
     * The database collection used by the model.
     *
     * @var string
     */
    protected $collection = 'users';

    /**
     * Get the name of the unique identifier for the user.
     *
     * @return string
     */
    public function getAuthIdentifierName()
    {
        return '_id';
    }

    /**
     * Get the unique identifier for the user.
     *
     * @return mixed
     */
    public function getAuthIdentifier()
    {
        return $this->_id;
    }

    /**
     * Get the password for the user.
     *
     * @return string
     */
    public function getAuthPassword()
    {
        return $this->password;
    }

    /**
     * Get the token value for the "remember me" session.
     *
     * @return string
     */
    public function getRememberToken()
    {
        return $this->remember_token;
    }


    /**
     * Set the token value for the "remember me" session.
     *
     * @param  string  $value
     */
    public function setRememberToken($value)
    {
        $this->remember_token = $value;
    }

    /**
     * Get the column name for the "remember me" token.
     *
     * @return string
     */
    public function getRememberTokenName()
    {
        return 'remember_token';
    }
}
```

Now, to log a user into your application, you may use the `auth()->attempt()` method.
You can use [any method regarding authentication](https://laravel.com/docs/5.2/authentication#included-authenticating).

## Queue Failed Job Provider

Mongolid Laravel replaces Laravel queue failed job provider to use a collection instead of a table. To configure the provider, update `failed` key on `queue.php` to include `collection` name:

```php
    'failed' => [
        'database' => 'mongodb',
        'collection' => 'failed_jobs',
    ],
```

**Note:** `database` key is irrelevant.

## Troubleshooting

**"PHP Fatal error: Class 'MongoDB\Client' not found in ..."**

The `MongoDB\Client` class is contained in the [MongoDB PHP Library](https://docs.mongodb.com/php-library/master/) and it requires [MongoDB driver for PHP](https://pecl.php.net/package/mongodb).
Here is an [installation guide](http://php.net/manual/en/mongodb.setup.php) for this driver.
The driver is a PHP extension written in C and maintained by [MongoDB](https://mongodb.com).
MongoLid and most other MongoDB PHP libraries utilize it in order to be fast and reliable.

**"Class 'MongoDB\Client' not found in ..." in CLI persists even with MongoDB driver installed.**

Make sure that the **php.ini** file used in the CLI environment includes the MongoDB extension. In some systems, the default PHP installation uses different **.ini** files for the web and CLI environments.

Run `php --ini` in a terminal to check the **.ini** that is being used.

To check if PHP in the CLI environment is importing the driver properly run `php -i | grep mongo` in your terminal. You should get output similar to:

```shell
$ php -i | grep mongo
mongodb
mongodb support => enabled
...
```

**"This package requires php >=7.0 but your PHP version (X.X.X) does not satisfy that requirement."**

The new (and improved) version 2.0 of Mongolid Laravel requires php7. If you are looking for the old PHP 5.x version, or other Laravel versions, head to the [v0.8 branch](https://github.com/leroy-merlin-br/mongolid-laravel/tree/v0.8-dev).

## License

MongoLid & MongoLid Laravel are free software distributed under the terms of the [MIT license](http://opensource.org/licenses/MIT).
Some of the code is based on the work of Taylor Otwell and contributors on [laravel/framework](https://github.com/laravel/framework), another free software distributed under the terms of the [MIT license](http://opensource.org/licenses/MIT).

## Additional information

Mongolid was proudly built by the [Leroy Merlin Brazil](https://github.com/leroy-merlin-br) team. [See all the contributors](https://github.com/leroy-merlin-br/mongolid/graphs/contributors).

Any questions, feel free to contact us.

Any issues, please [report here](https://github.com/leroy-merlin-br/mongolid-laravel/issues).
