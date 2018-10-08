[![Latest Stable Version](https://poser.pugx.org/phpcompatibility/phpcompatibility-symfony/v/stable.png)](https://packagist.org/packages/phpcompatibility/phpcompatibility-symfony)
[![Latest Unstable Version](https://poser.pugx.org/phpcompatibility/phpcompatibility-symfony/v/unstable.png)](https://packagist.org/packages/phpcompatibility/phpcompatibility-symfony)
[![License](https://poser.pugx.org/phpcompatibility/phpcompatibility-symfony/license.png)](https://github.com/PHPCompatibility/PHPCompatibilitySymfony/blob/master/LICENSE)
[![Build Status](https://travis-ci.org/PHPCompatibility/PHPCompatibilitySymfony.svg?branch=master)](https://travis-ci.org/PHPCompatibility/PHPCompatibilitySymfony)

# PHPCompatibilitySymfony

Using PHPCompatibilitySymfony, you can analyse the codebase of a project using any of the [Symfony polyfill libraries](https://github.com/symfony?utf8=?&q=polyfill), for PHP cross-version compatibility.


## What's in this repo ?

A set of rulesets for PHP_CodeSniffer to check for PHP cross-version compatibility issues in projects, while accounting for polyfills provided by the Symfony polyfill libraries.

These rulesets prevent false positives from the [PHPCompatibility standard](https://github.com/PHPCompatibility/PHPCompatibility) by excluding back-fills and poly-fills which are provided by those libraries.

Symfony Polyfill Library | Corresponding PHPCompatibility Ruleset | Includes
--- | --- | ---
[`polyfill-php54`](https://github.com/symfony/polyfill-php54) | `PHPCompatibilitySymfonyPolyfillPHP54` |
[`polyfill-php55`](https://github.com/symfony/polyfill-php55) | `PHPCompatibilitySymfonyPolyfillPHP55` | [`PHPCompatibilityPasswordCompat`](https://github.com/PHPCompatibility/PHPCompatibilityPasswordCompat)
[`polyfill-php56`](https://github.com/symfony/polyfill-php56) | `PHPCompatibilitySymfonyPolyfillPHP56` |
[`polyfill-php70`](https://github.com/symfony/polyfill-php70) | `PHPCompatibilitySymfonyPolyfillPHP70` | [`PHPCompatibilityParagonieRandomCompat`](https://github.com/PHPCompatibility/PHPCompatibilityParagonie)
[`polyfill-php71`](https://github.com/symfony/polyfill-php71) | `PHPCompatibilitySymfonyPolyfillPHP71` |
[`polyfill-php72`](https://github.com/symfony/polyfill-php72) | `PHPCompatibilitySymfonyPolyfillPHP72` |
[`polyfill-php73`](https://github.com/symfony/polyfill-php73) | `PHPCompatibilitySymfonyPolyfillPHP73` |

> About "Includes":
> Some polyfills have other polyfills as dependencies. If the PHPCompatibility project offers a dedicated ruleset for the polyfill dependency, that ruleset will be included in the ruleset for the higher level polyfill.
>
> For example:
> As the `polyfill-php70` library declares `random_compat` [as a dependency](https://github.com/symfony/polyfill-php70/blob/master/composer.json), the `PHPCompatibilitySymfonyPolyfillPHP70` ruleset includes the `PHPCompatibilityParagonieRandomCompat` ruleset.
>
> In practice, this means that if your project uses several polyfills, you can use the information in "Includes" to help you decide which rulesets to use.


## Requirements

* [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer).
    * PHP 5.3+ for use with [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) 2.3.0+.
    * PHP 5.4+ for use with [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) 3.0.2+.

    Use the latest stable release of PHP_CodeSniffer for the best results.
    The minimum _recommended_ version of PHP_CodeSniffer is version 2.6.0.
* [PHPCompatibility](https://github.com/PHPCompatibility/PHPCompatibility) 9.0.0+.
* [PHPCompatibilityParagonie](https://github.com/PHPCompatibility/PHPCompatibilityParagonie) 1.0.0+.
* [PHPCompatibilityPasswordCompat](https://github.com/PHPCompatibility/PHPCompatibilityPasswordCompat) 1.0.0+.


## Installation instructions

The only supported installation method is via [Composer](https://getcomposer.org/).

If you don't have a Composer plugin installed to manage the `installed_paths` setting for PHP_CodeSniffer, run the following from the command-line:
```bash
composer require --dev dealerdirect/phpcodesniffer-composer-installer:^0.4.4 phpcompatibility/phpcompatibility-symfony:*
composer install
```

If you already have a Composer PHP_CodeSniffer plugin installed, run:
```bash
composer require --dev phpcompatibility/phpcompatibility-symfony:*
composer install
```

Next, run:
```bash
vendor/bin/phpcs -i
```
If all went well, you will now see that the PHPCompatibility and a range of PHPCompatibilitySymfony and other PHPCompatibility standards are installed for PHP_CodeSniffer.


## How to use

Now you can use the following commands to inspect the code in your project for PHP cross-version compatibility:
```bash
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP54
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP55
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP56
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP70
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP71
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP72
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP73

# You can also combine the standards if your project uses several:
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP55,PHPCompatibilitySymfonyPolyfillPHP70,PHPCompatibilitySymfonyPolyfillPHP73
```

By default, you will only receive notifications about deprecated and/or removed PHP features.

To get the most out of the PHPCompatibilitySymfony rulesets, you should specify a `testVersion` to check against. That will enable the checks for both deprecated/removed PHP features as well as the detection of code using new PHP features.

For example:
```bash
# For a project which should be compatible with PHP 5.3 up to and including PHP 7.0:
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP56 --runtime-set testVersion 5.3-7.0

# For a project which should be compatible with PHP 5.4 and higher:
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP73 --runtime-set testVersion 5.4-
```

For more detailed information about setting the `testVersion`, see the README of the generic [PHPCompatibility](https://github.com/PHPCompatibility/PHPCompatibility#sniffing-your-code-for-compatibility-with-specific-php-versions) standard.


### Testing PHP files only

By default PHP_CodeSniffer will analyse PHP, JavaScript and CSS files. As the PHPCompatibility sniffs only target PHP code, you can make the run slightly faster by telling PHP_CodeSniffer to only check PHP files, like so:
```bash
./vendor/bin/phpcs -p . --standard=PHPCompatibilitySymfonyPolyfillPHP71 --extensions=php --runtime-set testVersion 5.3-
```

## License

All code within the PHPCompatibility organisation is released under the GNU Lesser General Public License (LGPL). For more information, visit https://www.gnu.org/copyleft/lesser.html


## Changelog

### 1.0.0 - 2018-10-07

Initial release of PHPCompatibilitySymfony containing rulesets covering the `polyfill-php*` libraries.