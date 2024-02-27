# Security Guide

## Settings files

Settings files should be avoided wherever possible, the only truly acceptable place for a settings file is within a plugin that is designed to only run on a single website.

If deemed necessary, this file should be named ```settings.php``` and stored within the plugin root.

There is an optimisation argument for settings files as each setting requires being loaded from the database, on the other hand there is a security implication for settings files as it is very easy to accidentally commit this file to GitHub. Additionally, settings files require the maintenance of a template for distribution too.

The performance implication can be somewhat mitigated with using add_option, by default add_option sets ```autoload``` to true, meaning the options are retrieved with a single query at load and cached for the remainder of the call.

Settings should ideally be stored in ```wp_options``` using ```add_option()``` and a corresponding settings page should be added, either to Campus Settings or WP Settings.