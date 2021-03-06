---
title: Errors & Logging
---

## Errors & Logging[](#errors-and-logging)

When you start a new PyroCMS project, error and exception handling is already configured for you through Laravel's default provisions.

For logging, Laravel utilizes the [Monolog](https://github.com/Seldaek/monolog) library, which provides support for a variety of powerful log handlers. Laravel configures several of these handlers for you, allowing you to choose between a single log file, rotating log files, or writing error information to the system log.



### Configuration[](#errors-and-logging/configuration)

This section will describe how to configure error logging for Pyro.



#### Error Detail[](#errors-and-logging/configuration/error-detail)

The `debug` option in your `config/app.php` configuration file determines how much information about an error is actually displayed to the user. By default, this option is set to respect the value of the `APP_DEBUG` environment variable, which is stored in your `.env` file.

For local development, you should set the `APP_DEBUG` environment variable to `true`. In your production environment, this value should always be `false`. If the value is set to `true` in production, you risk exposing sensitive configuration values to your application's end users.



#### Log Storage[](#errors-and-logging/configuration/log-storage)

Out of the box, PyroCMS supports writing log information exactly like Laravel. You can write to `single` files, `daily` files, the `syslog`, and the `errorlog`. To configure which storage mechanism Laravel uses, you should modify the `log` option in your `config/app.php` configuration file. For example, if you wish to use daily log files instead of a single file, you should set the `log` value in your `app` configuration file to `daily`:

    'log' => 'daily'



#### Maximum Daily Log Files[](#errors-and-logging/configuration/maximum-daily-log-files)

When using the `daily` log mode, PyroCMS will only retain five days of log files by default. If you want to adjust the number of retained files, you may add a `log_max_files` configuration value to your `app` configuration file:

    'log_max_files' => 30



#### Log Severity Levels[](#errors-and-logging/configuration/log-severity-levels)

When using Monolog, log messages may have different levels of severity. By default, PyroCMS writes all log levels to storage. However, in your production environment, you may wish to configure the minimum severity that should be logged by adding the `log_level` option to your `app.php` configuration file.

Once this option has been configured, Laravel will log all levels greater than or equal to the specified severity. For example, a default `log_level` of `error` will log **error**, **critical**, **alert**, and **emergency** messages:

    'log_level' => env('APP_LOG_LEVEL', 'error'),

Monolog recognizes the following severity levels - from least severe to most severe:

*   `debug`
*   `info`
*   `notice`
*   `warning`
*   `error`
*   `critical`
*   `alert`
*   `emergency`



### Custom Exceptions[](#errors-and-logging/custom-exceptions)

By default PyroCMS load the corresponding view from the Streams Platform matching the error code thrown when debugging is disabled.

For example if the system throws a 500 error and is not in debug mode then the `streams::errors/500` view will be loaded.

You can override these views a few different ways.



#### Publishing Streams Views[](#errors-and-logging/custom-exceptions/publishing-streams-views)

You can publish the entire Streams Platform resources by running the following command:

    php artisan streams:publish

You can then customize the `resources/views/errors` views to accommodate your needs.



#### Overriding from your theme[](#errors-and-logging/custom-exceptions/overriding-from-your-theme)

Your theme can override views manually and automatically. To automatically override error views in your theme simply create / copy the error views from Streams Platform to `resources/views/streams/errors`.



#### Defining theme overrides[](#errors-and-logging/custom-exceptions/defining-theme-overrides)

Lastly you can define view overrides manually within your theme class by defining the `$overrides` property:

    protected $overrides = [
        'streams::errors/500' => 'example.theme.test::custom/errors/500',
    ];

<div class="alert alert primary">**Pro Tip:** You can override any view in the same fashion as above.</div>
