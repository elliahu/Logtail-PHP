# Logtail PHP

Collect logs from your PHP projects, including Laravel, Symfony, CodeIgniter, CakePHP, Zend, and others. Logtail client library is built upon the well-known and highly popular logging library Monolog.

# Installation

You can install the Logtail client library using Composer as you would with any other PHP package. 

## Installing composer
If you are already familiar with the composer and have the composer already installed, you can proceed to the next step.

First, update the packages index and install the necessary requirements:
```bash
sudo apt update
sudo apt install wget php-cli php-zip unzip
```
Then, download the composer installer (composer installer will be downloaded to the current working directory):
```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```
Now, check if the donwload is valid (or corrupted):
```bash
HASH="$(wget -q -O - https://composer.github.io/installer.sig)"

php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```
You should see the following output:
```bash
Output:
Installer verified
```
Lastly, install the composer
```bash
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

## Installing Logtail to existing PHP project

To add Logtail client library to existing project, run the following command:
```bash
composer require logtail/monolog-logtail
```

## Installing Logtail example application

To install the example application, clone the current git repository or download all the files in the repository to the select working directory. In that directory, run the following command:
```bash
composer update
```
This command will install all dependencies from `composer.json` file and lock them in `composer.lock` file.

# Example application

The following script presents Logtail integration for PHP. 

```php
<?php
# Setting autoloader
require "vendor/autoload.php";

# Setting logger
use Monolog\Logger;
use Logtail\Monolog\LogtailHandler;

$logger = new Logger("logtail-source");
$logger->pushHandler(new LogtailHandler("98Q6bzjWk35nEjDixBPMzcfG"));

# Below you can see available methods that can be used to send logs to logtail.
# Each method corresponds to Monologs log level.
# You can also add additional context information in form of an array to any logging method and pass it as the
# second argument of the select method (as shown in the warn() method ).

# Send debug information using debug() method
$logger->debug("Logtail logger is ready!");

# Send information about interesting events using info() method
$logger->info("An interesting event occured!");

# Send information about normal but significant events using notice() method
$logger->notice("Sending notice");

# Send information about exceptional occurrences that might not be errors using warning() method
# You can also pass additional context information to any logging method in form of an array as the second argument
$logger->warning("Something is not quite right ...",[
    "additional_data" => [
        "item1" => "value1",
        "item2" => "value2"
    ]
]);

# Send information about runtime errors that might not require an immediate action using error() method
$logger->error("Oops! An runtime ERROR occurred!");

# Send information about critical conditions using critical() method
$logger->critical("Oh no! An critical event occurred!");

# Send an alert message about events for which action must be taken immediately using alert() method
$logger->alert("Something terrible happend! Imidiate action is required!");

# Send an emergency message about events that forced the application to crash using emergency() method 
$logger->emergency("Application just crashed! Imidiate action is required!");

echo "All done, you can check your logs in the control panel. \n";
```

## Running the example application
You can run the application by running the following command:
```bash
php index.php
```
You should see the following output:
```text
Output:
All done, you can check your logs in the control panel.
```

This will create and send a total of 8 log messages to the Logtail. Each message corresponds to a specific log level. Detail explanation of the code above follows below.

# Setup

First, if you haven’t done it already, require an autoloader file in which the class autoloader is defined. This autoloader will automatically require a class definition file whenever the class is called.

Then, you need to use the `Monolog\Logger` and `Logtail\Monolog\LogtailHandler` in which the logger is defined.

```php
# Setting autoloader
require "vendor/autoload.php";

# Setting logger
use Monolog\Logger;
use Logtail\Monolog\LogtailHandler;
```

Now it’s time to create a `Logger` instance and push a `LogtailHandler` handler instance on the top of the handler stack.

```php
$logger = new Logger("logtail-source");
$logger->pushHandler(new LogtailHandler("<source-token>"));
```

Don’t forget to change `<source-token>` to your actual token which you can find in the *Basic information* section when clicking on *Edit* on your select source.

Also, make sure that the `Logger` instance is accessible from all the branches of your application in which you wish to use the logger.

## Channels

When creating the `Logger` instance the `Logger()` constructor requires a `channel_string` representing the channel of your application.

You can split your application into imaginary channels that may correspond to functional components of your application and use the different channels for each of these components. This will allow you to filter your logs based on the channel. 

Creating multiple loggers for different channels is fairly easy:

```php
# Logger for shopping cart component
$cart_logger = new Logger("shoping-cart");
$cart_logger->pushHandler(new LogtailHandler("<source-token>"));

# Logger for payment component
$payment_logger = new Logger("payment");
$payment_logger->pushHandler(new LogtailHandler("<source-token>"));
```

Then you can filter your logs using the following search formula:

```
monolog.channel_string="shoping-cart"
```

This will only show logs from the `shoping-cart` channel.

# Logging

The `$logger` variable (instance of the `Logger` class) we have created in the setup has a number of methods used to send the logs to Logtail. Each of these methods corresponds to the Monolog log level. 

These levels are:

- **DEBUG** - Send debug information using the `debug()` method
- **INFO** - Send information about interesting events using the `info()` method
- **NOTICE** - Send information about normal but significant events using the `notice()` method
- **WARNING** - Send information about exceptional occurrences that might not be errors using the `warning()` method
- **ERROR** - Send information about runtime errors that might not require an immediate action using the `error()` method
- **CRITICAL** - Send information about critical conditions using the `critical()` method
- **ALERT** - Send an alert message about events for which action must be taken immediately using the `alert()` method
- **EMERGENCY** - Send an emergency message about events that forced the application to crash using the `emergency()` method

## Logging example

To send a log message of select log level, use the corresponding method. In this example, we will send the INFO level log and ERROR level log.

```php
# Send information about interesting events using info() method
$logger->info("An interesting event occured!");

# Send information about runtime errors that might not require an immediate action using error() method
$logger->error("Oops! An runtime ERROR occurred!");
```

This will output the following JSON data:

```json
{
   "dt":"2022-01-18 10:29:53.350 UTC",
   "level_string":"INFO",
   "message_string":"An interesting event occured!",
   "monolog":{
      "channel_string":"logtail-source",
      "context_string":"[]",
      "extra":{
         "file_string":"/path/to/file.php",
         "hostname_string":"USERAGENT",
         "line_integer":"28",
         "process_id_integer":"8677"
      }
   }
}

{
   "dt":"2022-01-18 10:29:53.550 UTC",
   "level_string":"ERROR",
   "message_string":"Oops! An runtime ERROR occurred!",
   "monolog":{
      "channel_string":"logtail-source",
      "context_string":"[]",
      "extra":{
         "file_string":"/path/to/file.php",
         "hostname_string":"USERAGENT",
         "line_integer":"43",
         "process_id_integer":"8677"
      }
   }
}
```

## Additional context information

You can attach additional context information to the actual log message which you can review later and which will help you troubleshoot your application by providing useful information. 

Let’s say that the application crashed when the user clicked on a button inside a shopping cart. In that case, it's useful to attach additional information such as the URL representing the user's location before the crash and the URL user tried to access when the crash happened. You can also attach the content of the shopping cart for further context. All this data might help you find the problem and fix it efficiently.

You can provide additional context information to any method in form of an array and pass it as the second argument.

```php
$logger->emergency("Application crash",[
    "additional_data" => [
        "current_URL" => "app/cart",
        "target_URL" => "app/payment",
        "username" => "someuser",
				"shopping_cart" => [
		            "item1" => "T-shirt",
		            "item2" => "Sneakers"
		        ]
    ]
]);
```

This log will output the following JSON data:

```json
{
   "dt":"2022-01-18 11:42:57.782 UTC",
   "level_string":"EMERGENCY",
   "message_string":"Application crash",
   "monolog":{
      "channel_string":"logtail-source",
      "extra":{
         "file_string":"/path/to/file.php",
         "hostname_string":"USERAGENT",
         "line_integer":"55",
         "process_id_integer":"8680"
      },
      "context":{
         "additional_data":{
            "current_URL_string":"app/cart",
            "shopping_cart":{
               "item1_string":"T-shirt",
               "item2_string":"Sneakers"
            },
            "target_URL_string":"app/payment",
            "username_string":"someuser"
         }
      }
   }
}
```