# Filebase
A Very Simple Flat File Database Storage.

[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/timothymarois/Filebase/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/timothymarois/Filebase/?branch=master)


## Installation

Use [Composer](http://getcomposer.org/) to install package.

Run `composer require timothymarois/filebase` or add to your main `composer.json` file.

## Usage

```php
// configuration to your database
$config = \Filebase\Database::config([
    'dir' => 'path/to/database/dir'
]);

$my_database = new \Filebase\Database($config);

// load up a single item "4325663.json"
$item = $my_database->get('4325663');

// Set Variables
$item->first_name = 'John';
$item->last_name  = 'Smith';

// This will either save current changes to the object
// Or it will create a new object using the id "4325663"
// Saves to "4325663.json"
$item->save();
```


## (1) Config Options

The config is *required* when defining your database. The options are *optional* as they have their own defaults.

```php
$config = \Filebase\Database::config([
    'dir'      => 'path/to/database/dir',
    'format'   => \Filebase\Format\Json::class
]);
```

|Name				|Type		|Default Value	    |Description												|
|---				|---		|---			         	|---														|
|`dir`				|string		|current directory          |The directory where the database files are stored. 	    |
|`format`			|object		|`\Filebase\Format\Json`   |The format class used to encode/decode data				|


## (2) Formatting

You can write your own or change the existing format class in the config. The methods in the class must be `static` and the class must implement `\Filebase\Format\FormatInterface`

The Default Format Class: `JSON`
```php
\Filebase\Format\Json::class
```


## (3) Get | FindAll

After you've loaded up your database config, then you can use the `get()` method to retrieve a single document of data.

```php
$item = $db->get('4325663');
```

You can also use `findAll()` to retrieve every document within the database directory.

```php
$items = $db->findAll();
```

## (4) Create | Update | Delete

As listed in the above example, its **very simple**. Use `$item->save()`, the `save()` method will either **Create** or **Update** an existing document by default. It will log all changes with `createdAt` and `updatedAt`. If you want to replace *all* data within a single document pass the new data in the `save($data)` method, otherwise don't pass any data to allow it to save the current instance.

```php

// SAVE or CREATE
// this will save the current data and any changed variables
// but it will leave existing variables that you did not modify unchanged.
// This will also create a document if none exist.
$item->title = 'My Document';
$item->save()

// This will replace all data within the document
// Allows you to reset the document and put in fresh data
// Ignoring any above changes or changes to variables, since
// This sets its own within the save method.
$item->save([
    'title' => 'My Document'
]);

// DELETE
// This will delete the current item
// This action can not be undone.
$item->delete();

```

You can change the date output format by sending in a php date format within the parameter of  `createdAt($date_format)` and `updatedAt($date_format)`.

```php
$created_at = $item->createdAt();

// by default Y-m-d H:i:s
echo $created_at;


$updated_at = $item->updatedAt();

// by default Y-m-d H:i:s
echo $updated_at;
```


## API (Methods)

```php
// sets the configuration
$db::config()

// gets a single item by ID (loads up in the instance)
$db->get()

// returns all the entries within the database instance
$db->findAll()


// saves the current item in instance
$item->save()

// deletes the current item in instance
$item->delete()

// returns the items as an array instead of object
$item->toArray()
```


## Why Filebase?

I originally built Filebase because I needed more flexibility, control over the database files, how they are stored, query filtration and to design with very intuitive API methods.

Inspired by [Flywheel](https://github.com/jamesmoss/flywheel) and [Flinetone](https://github.com/fire015/flintstone).

## Contributions

Accepting contributions and feedback. Send in any issues and pull requests.


## TODO

- Data Validation (new config option)
- Indexing (adding indexed "tag like")
- Querying (searching for fields, and pulling in multiple doc results)
- Auto-Increment ID or Create a hash ID
