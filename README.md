# civicrm/cli-tools

A bundle of CiviCRM-specific command-line tools. This specifically includes:

* `cv`: General purpose administration / Swiss Army knife
* `civistrings`: Scan source-code for translatable strings
* `civix`: Generate extensions
* `coworker`: Execute background jobs

This is intended for site-builders who have `composer`-based deployments (esp Drupal 9/10).
It supports workflows based on `composer require`, `composer update`, and `vendor-bin`.

## Usage

To add these tools to an existing `composer` build (e.g. Drupal 9/10 site), run:

```bash
composer require civicrm/cli-tools
```

This adds CiviCRM CLI tools to [composer's `vendor/bin` folder](https://getcomposer.org/doc/articles/vendor-binaries.md).

You can call commands through `composer exec` or `vendor/bin`:

```bash
## Example 1: Call cv through `composer exec`
composer exec cv api4 Contact.get +l 1

## Example 2: Call cv through `./vendor/bin`
./vendor/bin/cv api4 Contact.get +l 1

## Example 3: Add cv your PATH
PATH="/path/to/vendor/bin:$PATH"
cv api4 Contact.get +l 1
```

## In depth

* This is like a subset of `civicrm-buildkit`. It omits general development tools (`phpunit`), CMS-building tools (`drush`, `wp`),
  standard runtime environments (`min`, `max`), and autobuild sites (`drupal-clean`, `wp-demo`, etc). This is *only* the
  Civi-specific stuff that you would want to add on top of D9/D10 sites.
* The bridge here has 3 simple pieces:
    1. Download each executable via `composer-downloads-plugin` (e.g. `extern/cv.phar`).
    2. Add a stub script (e.g. `bin/cv`)
    3. Register the stub (`bin/cv`) for use with composer's `vendor/bin/`.
* This layout should be able to leverage composer's platform-specific wiring (e.g. Windows `.bat` files).
