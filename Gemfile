# frozen_string_literal: true

source "https://rubygems.org"

# Same Jekyll version and plugin set GitHub Pages uses, so the site renders
# identically on Cloudflare Pages. (GitHub Pages' built-in build ignores this file.)
gem "github-pages", group: :jekyll_plugins

# Standard-library gems that left Ruby's default gem set in 3.4 but are still
# required by Jekyll 3 (pinned by github-pages). Lets the site build on the
# Cloudflare build image's preinstalled Ruby 3.4 without compiling Ruby.
gem "csv"
gem "base64"
gem "bigdecimal"
