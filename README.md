mindplay/composer-locator
=========================

This Composer plugin provides a means of locating the installation path for a given Composer package name.

Use this to locate vendor package roots, e.g. when working with template files or other assets in a package.

You can think of this as a minimalist alternative to [puli](https://github.com/puli/repository) - rather than
abstracting repositories and resources through a complex virtual file system, I prefer to just use the physical
file system and standard PHP APIs.

Works for me. I like simple things. YMMV.

### Usage

Add to your `composer.json` file:

```json
{
    "require": {
        "mindplay/composer-locator": "^1"
    }
}
```

Running `composer install` or `composer update` will bootstrap your project with a simple, global function
that provides the installation path for a given Composer package name:

```php
$root_path = composer_path("mindplay/composer-locator");
```

If the specified package name is not found, the function throws a `RuntimeException`.

## Why?

Needing to know the root path of a package installation folder is quite a common requirement, such as when
you need to specify paths to template files or other assets.

The problem is that Composer itself offers no simple and reliable way to do that.

You can use reflection to get the path to a known class or interface from the package, and then `dirname()` up
from your `src` folder to the package installation root, but that approach is pretty clumsy and creates random
dependencies on arbitrary class/interface-names, just for the sake of locating a package root.

Even if you know the path of the vendor root folder, and the `{vendor}/{package}` folder name convention, there
is no guarantee that's always where packages are installed - something like [composer-installers](https://github.com/composer/installers)
or other [custom installers](https://github.com/akimsko/courier) could affect the installation paths.

Also, under test, when a package is the root/project package, of course the assumption about the vendor folder
is always going to be wrong.