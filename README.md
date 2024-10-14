# PSR Logger

This package provides a flexible PSR-3 compliant logging solution for your PHP application. It includes a logger with support for all PSR-3 log levels and optional logging to custom files or falling back to `error_log()` when no log file is provided.

## Features

- **PSR-3 Compliant**: Supports all PSR-3 log levels (`emergency`, `alert`, `critical`, `error`, `warning`, `notice`, `info`, `debug`).
- **File Logging**: Logs to a specified file, automatically creating the file and necessary directories if they do not exist.
- **Fallback to `error_log()`**: If no log file is specified, the package will fall back to PHP’s built-in `error_log()`.
- **Easy Setup and Use**: A simple static interface (`Log`) to use in your application.

## Installation

To install the package, you can use Composer:

```bash
composer require devuri/logger
```

Make sure to include Composer's autoloader in your project:

```php
require 'vendor/autoload.php';
```

## Usage

### Initialization

You can initialize the logger by specifying a log file. If no file is provided, it will fall back to `error_log()`.

```php
use Urisoft\Log;
use Urisoft\FileLogger;

// Initialize the logger with a custom log file
$logFile = __DIR__ . '/logs/app.log';
Log::init(new FileLogger($logFile));
```

### Logging Messages

You can log messages at various levels using the following methods:

```php
// Log an informational message
Log::info('Application started successfully.');

// Log an error with context (PSR-3 interpolation)
Log::error('Error occurred: {error}', ['error' => 'Database connection failed']);

// Log a warning message
Log::warning('Low disk space warning.');

// Log a critical error
Log::critical('Critical issue encountered in payment processing.');
```

### Log Levels

The package supports the following log levels:

- `Log::emergency($message, $context = [])`
- `Log::alert($message, $context = [])`
- `Log::critical($message, $context = [])`
- `Log::error($message, $context = [])`
- `Log::warning($message, $context = [])`
- `Log::notice($message, $context = [])`
- `Log::info($message, $context = [])`
- `Log::debug($message, $context = [])`

### Creating the Log File Automatically

If the log file or its directories do not exist, the logger will create them automatically:

```php
use Urisoft\Log;
use Urisoft\FileLogger;

$logFile = __DIR__ . '/logs/application.log';
Log::init(new FileLogger($logFile));  

Log::info('Logging system initialized.');
```

## Configuration

### Custom `error_log()`

If no log file is provided, the package will fall back to using PHP’s `error_log()` function. You can configure where `error_log()` writes by setting the `error_log` directive in your `php.ini` or using `ini_set()`:

```php
// Redirect error_log to a custom file
ini_set('error_log', __DIR__ . '/logs/error.log');

// Initialize logger without a file to fallback to error_log
Log::init(new FileLogger());
```

## License

This package is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

### Additional Notes:

- **Error Handling**: If the logger cannot create the log file or write to the specified location, it will throw a `RuntimeException` with a relevant message.
- **Log Rotation**: This package does not include log rotation functionality by default, so you may want to use external tools or another package to handle log rotation.
