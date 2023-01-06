# Description
Image based on official Alpine Linux image with ``php81fpm``, ``php81`` and ``bash`` installed via apk.
Entrypoint is ``entrypoint.bash`` BASH script executing following actions:
* check if there is includable configuration in ``/usr/local/etc/php81/conf.d`` (exit with error if there is none)
* create ``/srv/data/php81`` directory if not already present (``/srv/data`` is volume)
* create ``/srv/data/php81/log`` directory if not already present (``/srv/data`` is volume)
* launch ``php-fpm81`` with custom basic config (``-y``) and custom php ini (``-c``)

## Configuration
Custom basic configuration file provided as parameter to ``php-fpm81`` sets only
``pid``, ``error_log`` directives and then includes configuration files in directory ``/usr/local/etc/php81/conf.d`` matching wildcard ``*.fpm.conf``. In this directory file named ``php.fpm.ini`` has to be present for PHP configuration and also entrypoint depends on it. 

This directory is volume and it should be mount in Read-Only mode.

With above in mind it's obvious that all configuration is expected to be provided
by included configuration files. That's why entrypoint script fails with error
when there are none.

Distribution default directory ``/etc/php81`` and its files are preserved.

## Volumes
There are two volumes by default, one for configuration files and one for persistent data:
* ``/usr/local/etc/php81/conf.d`` - configuration files
* ``/srv/data`` - persistent data
