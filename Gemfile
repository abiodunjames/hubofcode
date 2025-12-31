# frozen_string_literal: true

source "https://rubygems.org"

# Jekyll core
gem "jekyll", "~> 4.2"

# GitHub Pages compatible gems
group :jekyll_plugins do
  # SEO & Discoverability
  gem "jekyll-seo-tag", "~> 2.8"           # Generates SEO meta tags
  gem "jekyll-sitemap", "~> 1.4"            # Generates sitemap.xml
  gem "jekyll-feed", "~> 0.17"              # Generates RSS/Atom feed
  
  # Content & Pagination
  gem "jekyll-paginate", "~> 1.1"           # Pagination support
  gem "jekyll-last-modified-at", "~> 1.3"   # Tracks last modification date
  
  # Social & Embedding
  gem "jemoji", "~> 0.13"                   # GitHub-style emoji support
  gem "jekyll-twitter-plugin", "~> 2.1"     # Embed tweets
  
  # Theming
  gem "jekyll-remote-theme", "~> 0.4"       # Use remote themes
  
  # Performance & Optimization
  gem "jekyll-minifier", "~> 0.1"           # Minify HTML, CSS, JS
  
  # Optional: Image optimization (uncomment if needed)
  # gem "jekyll-responsive-image", "~> 1.6"
end

# Windows and JRuby does not include zoneinfo files
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock http_parser.rb for JRuby
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

# Kramdown GFM parser
gem "kramdown-parser-gfm", "~> 1.1"
