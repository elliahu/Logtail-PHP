# Logging PHP application with Logtail

Collect logs from your PHP projects, including Laravel, Symfony, CodeIgniter, CakePHP, Zend, and others. Logtail client library is built upon the well-known and highly popular logging library Monolog. This guide also provides example PHP application which you can install and run with just a few simple steps. 

To learn more about Logtail in PHP projects [visit our docs](https://logtail-main.readme.io/docs/installing-logtail-1).

## Installing Logtail example application

To install the example application, clone the current git repository or download all the files in the repository to the select working directory. In that directory, run the following command:
```bash
composer update
```
This command will install all dependencies from `composer.json` file and lock them in `composer.lock` file.

## Running the example application
You can run the application by running the following command and don't forget to change the `<source-token>` to the actual source token which you can find in your source settings.
```bash
php index.php <source-token>
```
You should see the following output:
```text
All done, you can check your logs in the control panel.
```

This will create and send a total of 8 log messages to the Logtail. Each message corresponds to a specific log level. Detail explanation follows below.


