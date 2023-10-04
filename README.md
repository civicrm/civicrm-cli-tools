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
composer config repositories.repo-name vcs https://github.com/totten/civicrm-cli-tools
composer require civicrm/cli-tools
```

The tools will be available through `composer` conventions, e.g.

```
## Use "composer" to call "cv"
composer exec cv api4 Contact.get +l 1
```

or

```
## Add composer's bin dir to your PATH
PATH="$PWD/vendor/.bin:$PATH"
cv api4 Contact.get +l 1
```

## Next steps

This is a quick proof-of-concept. If feedback is positive, then some finalization steps would be:

1. Add `scripts/update` to quickly update `composer.json`. (This should probably read from `civicrm-buildkit:phars.json` or else `phive`.)
2. Move repo from `github.com/totten/` to `github.com/civicrm/`
3. Publish on packagist.
4. Simplify this README.

## In depth

* This is like a subset of `civicrm-buildkit`. It omits general development tools (`phpunit`), CMS-building tools (`drush`, `wp`),
  standard runtime environments (`min`, `max`), and autobuild sites (`drupal-clean`, `wp-demo`, etc). This is *only* the
  Civi-specific stuff that you would want to add on top of D9/D10 sites.
* The bridge here has 3 simple pieces:
    1. Download each executable via `composer-downloads-plugin` (e.g. `extern/cv.phar`).
    2. Add a stub script (e.g. `bin/cv`)
    3. Register the stub (`bin/cv`) for use with composer's `vendor/bin/`.
* This layout should be able to leverage composer's platform-specific wiring (e.g. Windows `.bat` files).
