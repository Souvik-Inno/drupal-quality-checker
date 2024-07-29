# Drupal Code Quality Checker
---

## Overview

This composer package will provide some basic code quality checks before committing code by using https://github.com/phpro/grumphp. Check out this [Lullabot article](https://www.lullabot.com/articles/how-enforce-drupal-coding-standards-git) for more details.

This has been customised from [vijaycs85/drupal-quality-checker](https://packagist.org/packages/vijaycs85/drupal-quality-checker) for Innoraft needs.


## Install

1. Add `innoraft/drupal-quality-checker` to `composer.json` or just run

    ```bash
    composer require --dev innoraft/drupal-quality-checker
    ```
2. Replace `grumphp.yml` in project's root directory (not Drupal root directory)
with `vendor/innoraft/drupal-quality-checker/grumphp.yml.dist`

That's it. Now, all tasks (listed below) run on every `git commit`.

>*Note:* As part of install, GrumPHP adds `pre-commit` hook to repository. Existing `pre-commit` might get [destroyed](https://github.com/phpro/grumphp/issues/416) when install/uninstall.

## Out of the box

1. [PHPCS](https://github.com/squizlabs/PHP_CodeSniffer) with Drupal standard.
2. [PHP Lint](http://www.icosaedro.it/phplint/)
3. [YAML Lint](http://www.yamllint.com/)
4. [Composer](https://github.com/composer/composer)
5. [Composer Normalize](https://github.com/ergebnis/composer-normalize)
6. [JSONLint](https://jsonlint.com/)
7. [Trufflehog](https://trufflesecurity.com/trufflehog/)

Long list of additional checks/validators available [here](https://github.com/phpro/grumphp/blob/master/doc/tasks.md#tasks-1).

## ESLint task Setup

1. Go to custom theme folder and add eslint package with npm
    ```bash
    npm install eslint --save-dev
    npm install eslint-config-airbnb --save-dev
    ```

2. Now run this command at project's root directory
    ```bash
    cp vendor/innoraft/drupal-quality-checker/eslintrc.json.dist .eslintrc.json
    ```
    For multisite run the following command
   ```bash
    cp vendor/innoraft/drupal-quality-checker/eslintrc.json.dist docroot/sites/<sitename>/themes/custom/<themename>/.eslintrc.json
    ```
4. Update bin key for eslint task under grumphp.yml file with relative address to eslint's bin file.

    Example: if your theme name is `drupal_theme` then change it to `web/themes/custom/drupal_theme/node_modules/.bin/eslint`

## Stylelint task Setup

1. Go to custom theme folder and add stylelint package with npm
    ```bash
    npm install stylelint --save-dev
    ```

2. Now run this command at project's root directory
    ```bash
    cp vendor/innoraft/drupal-quality-checker/stylelintrc.json.dist .stylelintrc.json
    ```
    For multisite run the following command
   ```bash
    cp vendor/innoraft/drupal-quality-checker/stylelintrc.json.dist docroot/sites/<sitename>/themes/custom/<themename>/.stylelintrc.json
    ```
3. For multisite run the following command
    ```bash
    npm install stylelint-config-standard
    ```
4. Update bin key for stylelint task under grumphp.yml file with relative address to stylelint's bin file.

    Example: if your theme name is `drupal_theme` then change it to `web/themes/custom/drupal_theme/node_modules/.bin/stylelint`

## Trufflehog task Setup

1. Create a new folder `tasks` at project's root level.

2. Now run this command at project's root directory
    ```bash
    cp vendor/innoraft/drupal-quality-checker/tasks/SecurityLeaks.php tasks
    ```

3. Now add the following code in your `composer.json` file which is present at project's root directory. ( For reference you can check `composer.json` file of this package )
    ```bash
    "autoload": {
        "psr-4": {
            "Innoraft\\QualityChecker\\Trufflehog\\": "tasks"
        }
    },
    ```
    Once the code is added in `composer.json` then execute this command
    ```bash
    composer dump-autoload
    ```
4. Install `trufflehog` ( If not already install ). You can execute the following command to achieve the same.
    ```bash
    curl -sSfL https://raw.githubusercontent.com/trufflesecurity/trufflehog/main/scripts/install.sh | sh -s -- -b $HOME/.local/bin
    ```

5. Please check and confirm if your `grumphp.yml` is updated with `vendor/innoraft/drupal-quality-checker/grumphp.yml.dist` ( Check and confirm if trufflehog tasks & service attribute is added in `grumphp.yml` file )

6. `Trufflehog` is now ready to sniff your secrets `:)`

## Multisite Setup
To set up for a multisite, follow these steps:

1. Use this [bash script](https://gist.github.com/sandipta18/d331643af7fa6b59c3ec8e0664e2844b).
2. Place the script outside the docroot.
3. To run the script, use `./multisite-setup.sh`.
