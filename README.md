FileQueue (Laravel4 Package)
=========
[![Latest Stable Version](https://poser.pugx.org/mcpruitt/file-queue/v/stable.png)](https://packagist.org/packages/mcpruitt/file-queue)
[![Latest Unstable Version](https://poser.pugx.org/mcpruitt/file-queue/v/unstable.png)](https://packagist.org/packages/mcpruitt/file-queue)
[![Total Downloads](https://poser.pugx.org/mcpruitt/file-queue/d/total.png)](https://packagist.org/packages/mcpruitt/file-queue)
[![Build Status](https://travis-ci.org/laravel/framework.png)](https://travis-ci.org/mcpruitt/file-queue)
[![ProjectStatus](http://stillmaintained.com/mcpruitt/FileQueue.png)](http://stillmaintained.com/mcpruitt/FileQueue)

A simple queue implementation for [laravel 4](https://github.com/laravel/laravel) that works with the filesystem. This can be used during development or when performance is not a concern (such as interacting with intermittent third party services).

## Features

* File based storage of queued jobs.
* Uses built-in [json_encode](http://php.net/json_encode) and [json_decode](http://php.net/json_deocde) for job storage.
* Works with closures as well as classes.
* Automatic retry logic on a dynamically increasing delay. 

## Quick Start

Simply add FileQueue to your `composer.json` file.

    "mcpruitt/file-queue":"dev-master"

You'll then need to run `composer install` to download it and have the class autoloader updated.

After composer has installed FileQueue you will need to add the service provider. Edit `app/config/app.php` and updated the `providers` array.

    'providers' => array(
      // Existing providers...
      'Mcpruitt\FileQueue\FileQueueServiceProvider',
     )

Finally you'll need to add the new queue to your `app/config/queue.php` and add a new entry to your `connections` array specifying the `file` driver.

    'file' => array(
	  'driver' => 'file'
    )

You can now use your [queue](http://four.laravel.com/docs/queues) like normal.

    Queue::push("DoSomething", array("Foo"=>"Bar"));

## Configuration Options

**driver** - This is the driver for the queue. This should be set to `file`.

**directory** - (default: `storage\FileQueue`) - This is the directory where job files should be stored.

**defaultqueue** (default: `default`) - This is the name of the default queue to place jobs under.

**bubleexceptions** (default: `false`) - If `true` exceptions that are thrown when a job is fired will be allowed to bubble up. If `false` exceptions will be caught and logged and the job will be released (with an incremented attempt count).

##### Example of default configuration:

    'file' => array(
	  'driver' => 'file',
	  'directory' => null,
      'defaultqueue' => 'default',
      'bubbleexceptions' => false
    )

## Directory Structure

By default the queue stores its job files in the `storage\FileQueue` folder. This folder can be overridden by specifying the `directory` option when configuring the queue. Within this directory another folder will be created for each queue name encountered (i.e. default). Within the name folder an inprocess folder will be created which holds jobs out of the standard queue folder while they are being processed.

## Job File Names

Job files are named in a uniform manner: `job-[JobName]-[TimeStamp]-[AttemptCount].json` i.e. `job-MyQueuedJob-12700000.0001-0.json`

