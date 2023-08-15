# Snakemake Docs Example

This is an example project that uses the [snakemake-plugin-docs-jekyll](https://github.com/snakemake/snakemake-plugin-docs-jekyll) theme
and customizes it for Snakemake plugins. Complete instructions for the theme are provided in the README [there](https://github.com/snakemake/snakemake-plugin-docs-jekyll).
The recommended approach to use this theme is to:

1. Clone or use it as a template
2. Move the entire root into a "docs" subdirectory
3. Turn on GitHub pages for this documentation directory

The above will serve documentation for your plugin directly from GitHub! If we update
the plugin templating, unless you pin a particular version, you will see updates for your
repository on fresh builds.

## Extra Notes

- Feel free to add other code or config files in the tutorial directory. 
- Any Jekyll path in the template can be over-ridden if you provide the same path locally.