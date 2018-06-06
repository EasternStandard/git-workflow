---
description: >-
  Boilerplate file for specifying Composer dependencies without touching
  Drupal's own composer.json
---

# Using composer.json outside of the docroot

In some cases it may be preferable to use a `docroot` folder \(or something similarly named\) to keep the Drupal core files as untouched as possible while still using Composer to specify module and theme dependencies. This can be done by creating a new `composer.json` file outside of the `docroot` folder. The following file is a boilerplate `composer.json` file that can be copied and modified as necessary:

{% code-tabs %}
{% code-tabs-item title="composer.json" %}
```javascript
{
    "name": "estrnstd/client-project",
    "type": "project",
    "require": {
        "composer/installers": "~1.0.24",
        "wikimedia/composer-merge-plugin": "^1.4",
        // Additional Drupal dependencies go here.
    },
    "minimum-stability": "dev",
    "prefer-stable": true,
    "config": {
        "preferred-install": "dist",
        "vendor-dir": "docroot/vendor"
    },
    "extra": {
        "merge-plugin": {
            "include": [
                "docroot/composer.json"
            ],
            "recurse": true,
            "replace": false,
            "merge-extra": false
        },
        "installer-paths": {
            "docroot/core": ["type:drupal-core"],
            "docroot/modules/contrib/{$name}": ["type:drupal-module"],
            "docroot/profiles/contrib/{$name}": ["type:drupal-profile"],
            "docroot/themes/contrib/{$name}": ["type:drupal-theme"],
            "docroot/drush/contrib/{$name}": ["type:drupal-drush"],
            "docroot/modules/custom/{$name}": ["type:drupal-custom-module"],
            "docroot/themes/custom/{$name}": ["type:drupal-custom-theme"]
        }
    },
    "repositories": [
        {
            "type": "composer",
            "url": "https://packages.drupal.org/8"
        }
    ],
    "scripts": {
        "post-autoload-dump": [
            "cd docroot && composer dump-autoload"
        ],
        "post-package-install": [
            "cd docroot && composer dump-autoload"
        ]
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Notes

* The `config/vendor-dir` option ensures two things: that you can reference existing composer packages from within the `docroot`, and that you don't create a second `vendor` folder outside the `docroot` as Composer installs various module dependencies.
* The `extra/merge-plugin/include` option should ONLY specify the root-level `composer.json` within the `docroot`, NOT additional files underneath. This is because the `docroot`'s `composer.json` already includes any additional `composer.json` files further down in the directory structure.
* The `scripts` option must include a way of running `composer dump-autoload` from within the `docroot`, otherwise Drupal will lose track of where the actual `vendor` folder is located.
* `"minimum-stability": "dev"`, `"prefer-stable": true`, and `config/preferred-install: "dist"` should all be specified to ensure compatibility with the stock `composer.json` file.
* If using a folder named something other than `docroot`, be sure to change it in the following locations:
  * `config/vendor-dir`
  * `extra/merge-plugin/include`
  * `extra/installer-paths`
  * `scripts/post-autoload-dump`
  * `scripts/post-package-install`

## Usage

Once the new `composer.json` file has been created and any relevant dependencies have been included in the `require` and `require-dev` keys, run `composer update --lock` BEFORE running `composer install`. This will ensure that all the deeper-level installed files are accounted for.

Once the initial setup has been complete, new module dependencies should be added outside of the `docroot` in this top-level `composer.json` via the normal method: `composer require drupal/module-name`.

When updating Drupal Core, the `composer.json` and `composer.lock` files can be safely overwritten with the new version from Drupal.org without fear of losing additional dependencies. It is recommended to run `composer install` from within the `docroot` folder after updating core, then running `composer update --lock` from outside the `docroot` folder to ensure that the latest dependencies are kept in sync.

