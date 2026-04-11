# frozen_string_literal: true

source "https://rubygems.org"

# We use ">=" here to allow your Ruby 4.0.2 version to work with the theme
gem "jekyll-theme-chirpy", ">= 7.5"

# Required for local preview on Ruby 3.0+
gem "webrick", "~> 1.8"

gem "html-proofer", "~> 5.0", group: :test

platforms :windows, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", :platforms => [:windows]