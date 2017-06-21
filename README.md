Laravel Rollbar
===============

Installation
------------

Install using composer:

```
composer require potsky/rollbar-laravel
```

Add Project Access Token `post_server_item` from Rollbar.com -> Settings -> Project Access Tokens to .env:

```
ROLLBAR_TOKEN=[your Rollbar project access token]
```

Add the service provider to the `'providers'` array in `config/app.php`:

```php
Rollbar\Laravel\RollbarServiceProvider::class,
```
    
If you only want to enable Rollbar reporting for certain environments you can conditionally load the service provider in your `AppServiceProvider`:

```php
if ($this->app->environment('production')) {
    $this->app->register(\Rollbar\Laravel\RollbarServiceProvider::class);
}
```

Configuration
-------------

This package supports configuration through the services configuration file located in `config/services.php`. All configuration variables will be directly passed to Rollbar:

```php
'rollbar' => [
    'access_token' => env('ROLLBAR_TOKEN'),
    'level' => env('ROLLBAR_LEVEL'),
    'person_fn' => function() {
		if (Auth::user()) {
			return [
				'id'       => strval(Auth::user()->id),
				'username' => strval(Auth::user()->name),
				'email'    => strval(Auth::user()->email),
			];
		} else {
			return [];
		}
	}
],
```

The level variable defines the minimum log level at which log messages are sent to Rollbar. For development you could set this either to `debug` to send all log messages, or to `none` to sent no messages at all. For production you could set this to `error` so that all info and debug messages are ignored.

Usage
-----

Fatal error and exceptions handled by Laravel are automatically sent to Rollbar. You do not need to add something in `app/Exceptions/Handler.php`.

Your other log messages will also be sent to Rollbar:

```php
\Log::debug('Here is some debug information');
```

### Context informaton

You can pass extra information:

```php
\Log::warning('Something went wrong', [
    'download_size' => 3432425235
]);
```
