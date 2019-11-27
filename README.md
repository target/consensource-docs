# Creating and updating the ConsenSource docs

Welcome to the GitHub repository for ConsenSource's public website. The docs are hosted at https://www.example.org.

We use Hugo to format and generate our website, the Docsy theme for styling and site structure, and ??? to manage the deployment of the site. Hugo is an open-source static site generator that provides us with templates, content organization in a standard directory structure, and a website generation engine. You write the pages in Markdown, and Hugo wraps them up into a website.

### Fork and clone the website repo and run a local website server

Follow the usual GitHub workflow to fork the repo on GitHub and clone it to your
local machine, then use your local repo as input to your Hugo web server:

1. **Fork** this repo
2. Clone your fork locally. This example uses HTTPS cloning:

    ```
    mkdir consensource
    cd consensource/
    git clone https://github.com/target/consensource-docs.git
    cd consensource-docs/
    ```

3. [Install Hugo](https://gohugo.io/getting-started/installing/) - e.g. `brew install hugo`. 
4. Start your website server. Make sure you run this command from the
   `/consensource-docs/` directory, so that Hugo can find the config files it needs:

    ```
    hugo server -D
    ```

5. You can access your website at 
  [http://localhost:1313/](http://localhost:1313/).

1. Continue with the usual GitHub workflow to edit files, commit them, push the
  changes up to your fork, and create a pull request. (There's some help with
  the GitHub workflow near the bottom of this page.)

1. While making the changes, you can preview them on your local version of the
  website at [http://localhost:1313/](http://localhost:1313/). Note that if you
  have more than one local git branch, when you switch between git branches the
  local website reflects the files in the current branch.

Useful docs:
- [User guide for the Docsy theme](https://www.docsy.dev/docs/getting-started/)
- [Hugo installation guide](https://gohugo.io/getting-started/installing/)
- [Hugo basic usage](https://gohugo.io/getting-started/usage/)
- [Hugo site directory structure](https://gohugo.io/getting-started/directory-structure/)
- [hugo server reference](https://gohugo.io/commands/hugo_server/)

## Menu structure

The site theme has one Hugo menu (`main`), which defines the top navigation bar. 
You can find and adjust the definition of the menu in the [site configuration 
file](https://github.com/target/consensource-docs/blob/master/config.toml). 

The left-hand navigation panel is defined by the directory structure under 
the 
[`docs` directory](https://github.com/target/consensource-docs/tree/master/content/docs). 

A `weight` property in the _front matter_ of each page determines the position 
of the page relative to the others in the same directory. The lower the weight,
the earlier the page appears in the section. A weight of 1 appears before a
a weight of 2, and so on. For example, see the front matter of the
[Overview](https://github.com/target/consensource-docs/blob/master/content/docs/Overview/_index.md)
page. The page front matter looks like this:

```
---
title: Overview
weight: 1
description: A high-level overview of the ConsenSource application
---
```

## Working with the theme

The theme files are in the 
[`themes/docsy` directory](https://github.com/target/consensource-docs/tree/master/themes/docsy).
**Do not change these files**, because they are overwritten each time we update
the website to a  later version of the theme, and your changes will be lost.

## Styling your content

The theme holds its styles in the 
[`assets/scss` directory](https://github.com/target/consensource-docs/tree/master/assets/scss).
**Do not change these files**, because they are overwritten each time we update
the website to a  later version of the theme, and your changes will be lost.

You can override the default styles and add new ones:

* In general, put your files in the project directory structure under `website` 
  rather than in the theme directory. Use the same file name as the theme does,
  and put the file in the same relative position. Hugo looks first at the file 
  in the main project directories, if present, then at the files under the theme 
  directory.
* You can update the ConsenSource docs site project variables in the 
  [`_variables_project.scss` file](https://github.com/target/consensource-docs/blob/master/assets/scss/_variables_project.scss).
  Values in that file override the
  [Docsy variables](https://github.com/target/consensource-docs/blob/master/themes/docsy/assets/scss/_variables_project.scss).
  You can also use `_variables_project.scss` to specify your own values for any 
  of the default 
  [Bootstrap 4 variables](https://getbootstrap.com/docs/4.0/getting-started/theming/).

The site's [front page](https://www.example.org/):

* See the [page source](https://github.com/target/consensource-docs/blob/master/content/_index.html).
* The CSS styles are in the 
  [project variables file](https://github.com/target/consensource-docs/blob/master/assets/scss/_variables_project.scss).
* The page uses the 
  [cover block](https://www.docsy.dev/docs/adding-content/shortcodes/#blocks-cover) 
  defined by the theme.
* The page also uses the 
  [linkdown block](https://www.docsy.dev/docs/adding-content/shortcodes/#blocks-link-down).

## Using Hugo shortcodes

Sometimes it's useful to define a snippet of information in one place and reuse
it wherever we need it. For example, we want to be able to refer to the minimum
version of various frameworks/libraries throughout the docs, without
causing a maintenance nightmare.

For this purpose, we use Hugo's "shortcodes". Shortcodes are similar to Django
variables. You define a shortcode in a file, then use a specific markup to
invoke the shortcode in the docs. That markup is replaced by the content of the
shortcode file when the page is built.

To create a shortcode:

1. Add an HTML file in  the `/website/layouts/shortcodes/` directory.
   The file name must be short and meaningful, as it determines the shortcode
   you and others use in the docs.

2. For the file content, add the text and HTML markup that should replace the
   shortcode markup when the web page is built.

To use a shortcode in a document, wrap the name of the shortcode in braces and
percent signs like this:

  ```
  {{% shortcode-name %}}
  ```

The shortcode name is the file name minus the `.html` file extension.

Useful Hugo docs:
- [Shortcode templates](https://gohugo.io/templates/shortcode-templates/)
- [Shortcodes](https://gohugo.io/content-management/shortcodes/)

## Attribution

This README was heavily influenced and based off of the README for [Kubeflow](https://github.com/kubeflow/website), another project using Hugo for their docs site.
