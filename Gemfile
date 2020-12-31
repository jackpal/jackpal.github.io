# Gemfile to setup Jekyll for use with Github pages hosting
# From https://gist.github.com/mdrmike/f59b3c92d0c403285cd3
# Based on
# - https://jekyllrb.com/docs/github-pages/
# - https://jekyllrb.com/docs/continuous-integration/

source 'https://rubygems.org'

require 'json'
require 'open-uri'
versions = JSON.parse(open('https://pages.github.com/versions.json').read)

gem 'github-pages', versions['github-pages']

## Optional setup for Travis-CI
#gem 'html-proofer'       # useful for Travis-CI
#gem 'scss_lint'          # useful for Travis-CI

# Google sitemap generator.

gem 'jekyll-sitemap'
