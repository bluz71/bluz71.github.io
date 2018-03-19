---
title: Rails specs using Capybara with headless Chrome
layout: default
comments: true
published: true
---

Rails specs using Capybara with headless Chrome
===============================================

There are a number of ways to drive end-to-end specs in Rails. For my own
personal project [Platters](https://github.com/bluz71/platters), which uses the
[RSpec](http://rspec.info/) testing framework, I settled on tried-and-true
[Capybara](https://teamcapybara.github.io/capybara/), and for specs that
required a JavaScript driver I used the
[Poltergeist](https://github.com/teampoltergeist/poltergeist) gem driving the
headless [PhantomJS](http://phantomjs.org/) browser engine.

This worked well enough even though PhantomJS did have some strange quirks.

Recently however, Google Chrome added in support for headless operation (sans
GUI). This headless Chrome capability prompted the PhantomJS lead to
[step down from the project](https://groups.google.com/forum/#!msg/phantomjs/9aI5d-LDuNE/5Z3SMZrqAQAJ).
From this announcement it seems pretty clear that PhantomJS is not a
technology to rely on for the long term.

Due to this PhantomJS uncertainty I converted my simple project over from
PhantomJS to headless Chrome including support for
[Travis CI](https://travis-ci.org/). After some grief, mainly related to
Travis CI, all is working fine and I am pleased with the final result.

Installation
============

First, please make sure you have a modern version of Chrome, at least Chrome
59, installed on your system.

Next you will need the
[ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/) package.

On macOS systems that have [Homebrew](https://brew.sh) installed:

```
brew install chromedriver
```

For Linux systems that have [Linuxbrew](http://linuxbrew.sh) installed:

```
brew install chromedriver
```

Otherwise for Linux systems, sans Linuxbrew, install ChromeDriver the
old-fashioned way:

```
wget http://chromedriver.storage.googleapis.com/2.32/chromedriver_linux64.zip
unzip chromedriver_linux64.zip
rm chromedriver_linux64.zip
sudo mv -f chromedriver /usr/local/bin/
sudo chmod +x /usr/local/bin/chromedriver
```

**NOTE**: Replace `2.32` with a recent version of ChromeDriver.

Configuration
=============

Replace `poltergeist` with `capybara-selenium` in your `Gemfile`, then `bundle
install`:

```ruby
gem 'capybara-selenium', group: :test
```

Next configure the Capybara JavaScript driver in `spec/rails_helper.rb`:

```ruby
require "selenium/webdriver"
Capybara.register_driver :chrome do |app|
  Capybara::Selenium::Driver.new(app, browser: :chrome)
end
Capybara.register_driver :headless_chrome do |app|
  capabilities = Selenium::WebDriver::Remote::Capabilities.chrome(chromeOptions: {args: %w(headless)})
  Capybara::Selenium::Driver.new(app, browser: :chrome, desired_capabilities: capabilities)
end
Capybara.javascript_driver = :headless_chrome
```

Note, this configuration provides both headless and standard Chrome drivers. In
the `javascript_driver` line replace `:headless_chrome` with `:chome` if one
desires a visible browser whilst running JavaScript specs.

That is all, running specs with `bundle exec rspec` should now run headless
Chrome for tests involving JavaScript.

Travis CI configuration
=======================

Configuring my application to successfully use headless Chrome on Travis CI
proved to be a bit more challenging mainly due to the absence of ChromeDriver.

This, possibly non-optimal, configuration snippet proved successful with my
application on Travis CI. In `.travis.yml`:

```
addons:
  - chrome: stable

before_install:
  - wget http://chromedriver.storage.googleapis.com/2.32/chromedriver_linux64.zip
  - unzip chromedriver_linux64.zip
  - rm chromedriver_linux64.zip
  - sudo mv -f chromedriver /usr/local/bin/
  - sudo chmod +x /usr/local/bin/chromedriver
  - google-chrome-stable --headless --no-sandbox
```

**NOTE**: Replace `2.32` with a recent version of ChromeDriver.

Be aware, the above listed Travis CI configuration is just a subset of a full
Travis CI configuration, my application's full Travis CI configuration is listed
[here](https://github.com/bluz71/platters/blob/master/.travis.yml).

My specs now run successfully on Travis CI using headless Chrome.
