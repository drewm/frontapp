FrontApp API client
===================

Super-simple, minimum abstraction FrontApp API wrapper, in PHP.

In the style of my [MailChimp wrapper](https://github.com/drewm/mailchimp-api), this lets you get from the FrontApp API docs to the code as directly as possible.

Requires PHP 5.4.

[![Build Status](https://travis-ci.org/drewm/frontapp.svg?branch=master)](https://travis-ci.org/drewm/frontapp)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/drewm/frontapp/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/drewm/frontapp/?branch=master)
[![Packagist](https://img.shields.io/packagist/dt/drewm/frontapp.svg?maxAge=2592000)](https://packagist.org/packages/drewm/frontapp)

Installation
------------

You can install frontapp using Composer:

```
composer require drewm/frontapp
```

You will then need to:
* run ``composer install`` to get these dependencies added to your vendor directory
* add the autoloader to your application with this line: ``require("vendor/autoload.php")``

Examples
--------

Start by `use`-ing the class and creating an instance with your API key

```php
use \DrewM\FrontApp\FrontApp;

$FrontApp = new FrontApp('abc123abc123abc123abc123abc123');
```

Then, list all your inboxes (with a `get` on the `inboxes` method)

```php
$result = $FrontApp->get('inboxes');

print_r($result);
```

Create an inbox (with a `post` to the `inboxes` method):

```php
$list_id = 'b1234346';

$result = $FrontApp->post("inboxes", [
				'name' => 'Support'
			]);

print_r($result);
```

Update a channel member with more information (using `patch` to update):

```php
$channel_id = 'cha_123';

$result = $FrontApp->patch("channels/$channel_id", [
				'settings' => [ 'webhook_url' => 'https://example.com/hook' ]
			]);

print_r($result);
```

Remove a contact using the `delete` method:

```php
$contact_id = '1234';

$FrontApp->delete("contacts/$contact_id");
```

Quickly test for a successful action with the `success()` method:

```php
$result = $FrontApp->get('inboxes');

if ($FrontApp->success()) {
	print_r($result);	
} else {
	echo $FrontApp->getLastError();
}
```

Troubleshooting
---------------

To get the last error returned by either the HTTP client or by the API, use `getLastError()`:

```php
echo $FrontApp->getLastError();
```

For further debugging, you can inspect the headers and body of the response:

```php
print_r($FrontApp->getLastResponse());
```

If you suspect you're sending data in the wrong format, you can look at what was sent to FrontApp by the wrapper:

```php
print_r($FrontApp->getLastRequest());
```
