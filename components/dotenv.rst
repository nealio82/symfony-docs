.. index::
   single: Dotenv
   single: Components; Dotenv

The Dotenv Component
====================

    The Dotenv Component parses ``.env`` files to make environment variables
    stored in them accessible via ``getenv()``, ``$_ENV`` or ``$_SERVER``.

Installation
------------

.. code-block:: terminal

    $ composer require --dev symfony/dotenv

.. include:: /components/require_autoload.rst.inc

Usage
-----

Sensitive information and environment-dependent settings should be defined as
environment variables (as recommended for `twelve-factor applications`_). Using
a ``.env`` file to store those environment variables eases development and CI
management by keeping them in one "standard" place and agnostic of the
technology stack you are using (Nginx vs PHP built-in server for instance).

.. note::

    PHP has a lot of different implementations of this "pattern". This
    implementation's goal is to replicate what ``source .env`` would do. It
    tries to be as similar as possible with the standard shell's behavior (so
    no value validation for instance).

Load a ``.env`` file in your PHP application via ``Dotenv::load()``::

    use Symfony\Component\Dotenv\Dotenv;

    $dotenv = new Dotenv();
    $dotenv->load(__DIR__.'/.env');

    // You can also load several files
    $dotenv->load(__DIR__.'/.env', __DIR__.'/.env.dev');

Given the following ``.env`` file content:

.. code-block:: terminal

    # .env
    DB_USER=root
    DB_PASS=pass

Access the value with ``getenv()`` in your code::

    $dbUser = getenv('DB_USER');
    // you can also use ``$_ENV`` or ``$_SERVER``

The ``load()`` method never overwrites existing environment variables. Use the
``overload()`` method if you need to overwrite them::

    // ...
    $dotenv->overload(__DIR__.'/.env');

You should never store a ``.env`` file in your code repository as it might
contain sensitive information; create a ``.env.dist`` file with sensible
defaults instead.

.. note::

    Symfony Dotenv can be used in any environment of your application:
    development, testing, staging and even production. However, in production
    it's recommended to configure real environment variables to avoid the
    performance overhead of parsing the ``.env`` file for every request.

As a ``.env`` file is a regular shell script, you can ``source`` it in your own
shell scripts:

.. code-block:: terminal

    source .env

Add comments by prefixing them with ``#``:

.. code-block:: terminal

    # Database credentials
    DB_USER=root
    DB_PASS=pass # This is the secret password

Use environment variables in values by prefixing variables with ``$``:

.. code-block:: terminal

    DB_USER=root
    DB_PASS=${DB_USER}pass # Include the user as a password prefix

Embed commands via ``$()`` (not supported on Windows):

.. code-block:: terminal

    START_TIME=$(date)

.. note::

    Note that using ``$()`` might not work depending on your shell.

.. _Packagist: https://packagist.org/packages/symfony/dotenv
.. _twelve-factor applications: http://www.12factor.net/
