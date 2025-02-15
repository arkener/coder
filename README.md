# Coder

[![Automated tests](https://github.com/pfrenssen/coder/workflows/Tests/badge.svg)](https://github.com/pfrenssen/coder/actions)

Coder is a library for automated Drupal code reviews and coding standard fixes. It
defines rules for [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)

Built-in support for:
- "Drupal": Coding Standards https://www.drupal.org/coding-standards
- "DrupalPractice": Best practices for Drupal module development

Online documentation: https://www.drupal.org/node/1419980

Note that Javascript support has been removed. To check and fix Javascript files
please use [ESLint](http://eslint.org/) and see the
[Drupal ESLint](https://www.drupal.org/node/1955232) documentation.


## Global installation

First, make sure Composer is installed correctly:

    which composer

If you get composer not found or similar, follow Composer's installation
instructions.

Install Coder (8.3.x) in your global Composer directory in your home directory
(`~/.config/composer`):

    composer global require drupal/coder

To make the `phpcs` and `phpcbf` commands available globally, add the Composer
bin path to your `$PATH` variable in `~/.profile`, `~/.bashrc` or `~/.zshrc`:

    export PATH="$PATH:$HOME/.config/composer/vendor/bin"


## Usage

Check Drupal coding standards

    phpcs --standard=Drupal --extensions=php,module,inc,install,test,profile,theme,css,info,txt,md,yml /file/to/drupal/example_module

Check Drupal best practices

    phpcs --standard=DrupalPractice --extensions=php,module,inc,install,test,profile,theme,css,info,txt,md,yml /file/to/drupal/example_module

Automatically fix coding standards

    phpcbf --standard=Drupal --extensions=php,module,inc,install,test,profile,theme,css,info,txt,md,yml /file/to/drupal/example_module


## Local installation in your Drupal project

For collaborating developer teams it makes sense to lock a Coder version with
your other development dependencies locally. In your project where you have your
composer.json file execute this:

    composer require --dev drupal/coder

Then you can run Coder on your custom module code like this:

    ./vendor/bin/phpcs --standard=Drupal --extensions=php,module,inc,install,test,profile,theme,css,info,txt,md,yml web/modules/custom

To simplify execution it is highly recommended to use a settings file, see below.


## Store settings in a phpcs.xml.dist file

In order to save and commit your PHPCS settings to Git you can use a
phpcs.xml.dist file in your project like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ruleset name="example">
  <description>PHP CodeSniffer configuration for example development.</description>
  <!-- Check all files in the current directory and below. -->
  <file>.</file>
  <arg name="extensions" value="php,module,inc,install,test,profile,theme,css,info,txt,md,yml"/>
  <!-- Change this value to 7 if you want to check Drupal 7 code. -->
  <config name="drupal_core_version" value="8"/>

  <!-- If you have Coder installed locally then you can reference the Drupal
  standards with relative paths. Otherwise simply use "Drupal" and
  "DrupalPractice. -->
  <rule ref="../vendor/drupal/coder/coder_sniffer/Drupal">
    <!-- Example how you would disable a rule you are not compliant with yet:
    <exclude name="Drupal.Commenting.Deprecated"/>
    -->
  </rule>
  <rule ref="../vendor/drupal/coder/coder_sniffer/DrupalPractice"/>

  <!-- Example how you would disable an external rule you do not like:
  <rule ref="PEAR.Functions.ValidDefaultValue.NotAtEnd">
    <severity>0</severity>
  </rule>
  -->
</ruleset>
```

Then you can invoke phpcs without any options and it will read phpcs.xml.dist
from the current directory. This can also be useful for Continuous Integration
setups. Example:

    ./vendor/bin/phpcs -p


## Working with Editors

Drupal Code Sniffer can be used with various editors.

Editors:

- [Atom](https://www.drupal.org/node/1419996)
- [Eclipse](https://www.drupal.org/node/1420004)
- [Komodo](https://www.drupal.org/node/1419996)
- [Netbeans](https://www.drupal.org/node/1420008)
- [PhpStorm](https://www.jetbrains.com/help/phpstorm/php-code-sniffer.html)
- [Sublime Text](https://www.drupal.org/node/1419996)
- [Vim](https://www.drupal.org/node/1419996)
- [Visual Studio Code (VSCode)](https://www.drupal.org/node/1419996)


## Automated Testing (PHPUnit + PHPCS + PHPStan)

Coder Sniffer comes with a PHPUnit test suite to make sure the sniffs work correctly.
Use Composer to install the dependencies:

    composer install

Then execute the tests:

    ./vendor/bin/phpunit

Then execute the coding standards checker on Coder itself:

    ./vendor/bin/phpcs

Then execute static analysis:

    ./vendor/bin/phpstan


## Contributing

1. Make sure an issue exists at https://www.drupal.org/project/issues/coder
2. Create a [Pull Request](https://help.github.com/articles/using-pull-requests/) against https://github.com/pfrenssen/coder
3. Post a link to the pull request to the issue on drupal.org and set the issue to
   "needs review"

Thank you!


## Maintainers

[//]: # cspell:ignore Pieter Frenssen Welford

- Pieter Frenssen, https://www.drupal.org/u/pfrenssen
- Michael Welford, https://www.drupal.org/u/mikejw
- Klaus Purer, https://www.drupal.org/u/klausi


## Credits

Greg Sherwood and Squiz Pty Ltd, many sniffs are modified copies of their original
work on [PHPCS](https://github.com/squizlabs/PHP_CodeSniffer).
