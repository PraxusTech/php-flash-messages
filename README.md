# PHP Flash Messages

A modern PHP library for session-based flash messages, compatible with Bootstrap alert styles.

Forked and extended from [plasticbrain/php-flash-messages](https://github.com/plasticbrain/php-flash-messages).

---

## Requirements

- PHP >= 5.4.0
- An active PHP session

---

## Installation

Via Composer:

```bash
composer require praxustech/php-flash-messages
```

---

## Basic Usage

```php
<?php

// Start a session before using the library
session_start();

require_once 'vendor/autoload.php';

use PraxusTech\FlashMessages\FlashMessages;

$flash = new FlashMessages();

// Add messages
$flash->info('This is an info message.');
$flash->success('Operation completed successfully!');
$flash->warning('Please review your input.');
$flash->error('Something went wrong.');

// Display all messages
$flash->display();
```

---

## Message Types

| Method | Type constant | CSS class applied |
|---|---|---|
| `info()` | `FlashMessages::INFO` | `alert-info` |
| `success()` | `FlashMessages::SUCCESS` | `alert-success` |
| `warning()` | `FlashMessages::WARNING` | `alert-warning` |
| `error()` | `FlashMessages::ERROR` | `alert-danger` |

---

## Adding Messages

### Shorthand methods

```php
$flash->info('Your profile has been updated.');
$flash->success('File uploaded successfully.');
$flash->warning('Your session will expire soon.');
$flash->error('Invalid credentials.');
```

### Generic method

```php
// $flash->add($message, $type, $redirectUrl, $sticky)
$flash->add('Custom message', FlashMessages::SUCCESS);
```

---

## Redirecting After Adding a Message

Pass a URL as the second argument to any method. The user will be redirected immediately after the message is queued in the session.

```php
$flash->error('You must be logged in.', '/login');

$flash->success('Profile saved!', '/dashboard');

// Also works with the generic add() method
$flash->add('Account created!', FlashMessages::SUCCESS, '/welcome');
```

---

## Sticky Messages

Sticky messages do not show a close button. Use this for important notices that should stay visible.

```php
// Using the sticky() method (defaults to INFO type)
$flash->sticky('This is a sticky info message.');

// Pass a type as the third argument
$flash->sticky('Critical error occurred.', null, FlashMessages::ERROR);

// Or use the $sticky parameter on any method
$flash->warning('Maintenance scheduled tonight.', null, true);
$flash->error('Account suspended.', '/contact', true);
```

---

## Displaying Messages

### Display all messages

```php
$flash->display();
```

### Display only a specific type

```php
$flash->display(FlashMessages::ERROR);
```

### Display multiple specific types

```php
$flash->display([FlashMessages::ERROR, FlashMessages::WARNING]);
```

### Control display order

By default, messages are displayed in this order: error, warning, success, info. Pass an array to override the order:

```php
$flash->display([FlashMessages::SUCCESS, FlashMessages::INFO, FlashMessages::ERROR, FlashMessages::WARNING]);
```

### Return instead of echo

Pass `false` as the second argument to get the HTML as a string instead of printing it:

```php
$html = $flash->display(null, false);
echo $html;
```

---

## Checking for Messages

```php
// Check if there are any error messages
if ($flash->hasErrors()) {
    // Handle errors
}

// Check if there are messages of any type
if ($flash->hasMessages()) {
    // There are pending messages
}

// Check for a specific type
if ($flash->hasMessages(FlashMessages::SUCCESS)) {
    // There are success messages
}
```

---

## Customization

All customization methods return `$this`, so they can be chained.

### Custom HTML wrapper

The wrapper receives two `%s` placeholders: the CSS class and the message content.

```php
$flash->setMsgWrapper("<div class='%s'><p>%s</p></div>\n");
```

### Prepend/append content to messages

```php
$flash->setMsgBefore('<strong>Notice:</strong> ');
$flash->setMsgAfter(' <em>(please read)</em>');
```

### Custom close button

```php
$flash->setCloseBtn('<button class="btn-close" data-bs-dismiss="alert"></button>');
```

### Custom CSS classes

```php
// Change the base CSS class applied to all messages
$flash->setMsgCssClass('alert dismissible fade show');

// Change the CSS class for sticky messages
$flash->setStickyCssClass('pinned');

// Map message types to custom CSS classes
$flash->setCssClassMap(FlashMessages::ERROR, 'my-error-class');

// Or update multiple at once using an array
$flash->setCssClassMap([
    FlashMessages::SUCCESS => 'toast-success',
    FlashMessages::ERROR   => 'toast-error',
]);
```

### Chaining customizations

```php
$flash
    ->setMsgWrapper("<div class='%s'>%s</div>\n")
    ->setMsgCssClass('alert show')
    ->setCloseBtn('<button class="btn-close" data-bs-dismiss="alert"></button>');
```

---

## License

Licensed under the [Apache 2.0](LICENSE) and MIT licenses.

Original work by [Mike Everhart](http://mikeeverhart.net).
Extended by [Joao Pedro Matias](mailto:joaopedrord2001@gmail.com) and [Bruna Gomes](mailto:brunaclegomes10@gmail.com).
