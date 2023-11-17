# Plugin Release Guide

This document applies to the release of WordPress plugins to be used on sites that utilise ```Real Group Plugin Repository``` to manage updates.

Since the introduction of the Real Group WordPress Plugin updater, some guidance is required on how to create releases for code updates. This is to ensure compatability with the WordPress Plugin updater and to ensure that plugins are successfully updated without causing unwanted issues on live sites.

## Release structure

The release zip can contain any of the following folder structures

* Plugin files at the base level of the zip
* Plugin files stored in a ```src``` folder at the top level of the zip
* Plugin files stored in a ```build``` folder at the top level of the zip

## Version numbering

RealGroup loosly follows semantic versioning.

Version are to contain 3 period separated numbers

* Major release
  * This is usually code / site breaking features with limited backwards compatability
* Feature release
  * The additon of a new feature or a non breaking change to an existing feature
* Bug fix
  * Fixing a bug related to an existing feature

Additionally, if multiple branches exist, a version number may be suffixed with a hyphen and a branch descriptor.

For example, version ```1.4.28``` of a plugin, contains a PHP 8.2 specific version, the version number for this would be ```1.4.28-PHP8.2```

## Dependencies

If a plugin relies on external dependencies, such as packages pulled in from composer. A "compiled" version of the plugin must be created and zipped. 

This file MUST be uploaded as the release file.

External dependencies, loaded in via external URL's or content delivery networks should ideally be avoided. Instead, external libraries should be included in the release package and hosted locally to ensure they remain available.

### Important note

When creating a release zip, there is a very important structure to follow - The zip contents MUST be layed out with a single folder at the root of the zip.

Failure to follow this structure can and will result in failed updates!

```
my_plugin.zip
    my_plugin-1.2.15
        controllers
        models
        vendor
        view
        plugin.php
```

The folder ```my_plugin-1.2.15``` can be named any valid folder name, although it is recommended to use the structure ```plugin_name``` - ```version```.

Contents within ```my_plugin-1.2.15``` can include any combination of files and / or folders - this content forms the installation content for the plugin.

## Minification

Any Javascript and / or CSS within a plugin is to be minified before release. A simple define can be used within the plugin header to enable / disable minification for development.

```if (!defined('PLUGIN_NAME_DEV')) define('PLUGIN_NAME_DEV', true);```

Javascript and CSS can then be enqueued to handle development mode:

```
wp_enqueue_script(
    'plugin_tag', 
    PLUGIN_URL.'js/script'.(PLUGIN_NAME_DEV ? '' : '.min').'.js', 
    array(), 
    PLUGIN_NAME_VERSION
);

wp_enqueue_style(
    'plugin_tag', 
    PLUGIN_URL.'css/style'.(PLUGIN_NAME_DEV ? '' : '.min').'.css', 
    array(), 
    PLUGIN_NAME_VERSION
);
```

If you wish to utilise development mode within Javascript, for example, to enable / disable console logs:

```
wp_localize_script(
    'plugin_tag',
    'plugin_name_conf',
    array(
        'dev_mode => PLUGIN_NAME_DEV,
    )
);
```

This enables the development mode setting to be accessed in Javascript with ```plugin_tag.dev_mode```

## Hard coded configuration

Although highly discouraged, occasionally a plugin will require hard coded settings - This is often the case for site specific plugins.

Where possible and economical, settings should be moved to a configuration screen within WordPress.

If this is not possible, the following file names are reserved and will never be deleted from the server via automated updates

* settings.php
* settings.json
* config.php
* config.json

If a settings file is required with the plugin, this file must not be included in the release package as it will cause the settings / config file to be overritten.

Instead an ```example``` suffixed file must be included with example settings - E.g. ```settings.example.php```.

Any live settings files should be included in .gitignore to ensure they are not committed.

