# GENESIS Website Repository

This repository contains the source files for the official GENESIS website.  
The site is built using [Jekyll](https://jekyllrb.com/) and hosted via [GitHub Pages](https://pages.github.com/).  
All content is written in Markdown and organized for clarity, maintainability, and community contribution.

## Purpose

This project serves as the foundation of the GENESIS MD software’s official website.  
It provides essential documentation, tutorials, feature introductions, updates, and useful resources for users and developers of the GENESIS molecular dynamics program.

## Structure

- Most contents are written in Markdown (`*.md`)
- Layout and styling are managed via Jekyll templates and Liquid
- Custom collections such as tutorials, updates, and documentation are defined in `_config.yml`

## How to Contribute

We welcome contributions from the community. If you:
- Find a typo or outdated content
- Have suggestions for improvements
- Want to contribute new pages or tutorials

Please open an [Issue](https://github.com/genesis-release-r-ccs/genesis_website/issues) or submit a [Pull Request](https://github.com/genesis-release-r-ccs/genesis_website/pulls).  
Make sure to follow the directory structure and formatting conventions used across the project.

## Local Preview

To preview the website locally (you'll need Ruby and Bundler installed):

```bash
bundle install
bundle exec jekyll serve
```

