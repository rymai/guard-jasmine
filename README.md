# Guard::Jasmine [![Build Status](https://secure.travis-ci.org/netzpirat/guard-jasmine.png)](http://travis-ci.org/netzpirat/guard-jasmine)

Guard::Jasmine automatically tests your Jasmine specs when files are modified.

Tested on MRI Ruby 1.8.7, 1.9.2, REE and the latest versions of JRuby & Rubinius.

If you have any questions please join us on our [Google group](http://groups.google.com/group/guard-dev) or on `#guard`
(irc.freenode.net).

## Highlights

* Continuous testing based on file modifications by [Guard][], manifold configurable rules
with RegEx and Ruby.

* Fast headless testing on [PhantomJS][], a full featured WebKit browser with native support for
various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG.

* With Rails 3.1 you can write your [Jasmine][] specs in [CoffeeScript][] also, fully integrated into the
[Rails 3.1 asset pipeline][] with [Jasminerice][].

* Runs on Mac OS X, Linux and Windows.

## How it works

1. Configure your Jasmine based JavaScript/CoffeeScript specs with Jasminerice in the asset pipeline, serve it over
the normal Jasmine Spec Runner. (With Rails 2 & 3 you configure your plain JavaScript Jasmine specs with [the Jasmine Gem][].)

2. You configure Guard to trigger certain specs based on file modifications.

3. Guard uses PhantomJS to request the Jasmine Spec Runner headless and notifies you of the result in the terminal and
optionally over system notifications like Growl, Libnotify or Notifu.

## Install

### Guard and Guard::Jasmine

Please be sure to have [Guard][] installed.

Install the gem:

    $ gem install guard-jasmine

Add it to your `Gemfile`, preferably inside the development group:

    gem 'guard-jasmine'

Add guard definition to your `Guardfile` by running this command:

    $ guard init jasmine

### Rails 3.1

With Rails 3.1 you can write your Jasmine specs in addition to JavaScript with CoffeeScript, fully integrated into the
Rails 3.1 asset pipeline with Jasminerice.

Please read the detailed installation and configuration instructions at [Jasminerice][].

In short, you add it to your `Gemfile`:

    group :development, :test do
      gem 'jasmine'
    end

and add a route for the Jasmine Test Runner to `config/routes.rb`:

    if ["development", "test"].include? Rails.env
      mount Jasminerice::Engine => "/jasmine"
    end

Next you create the directory `spec/javascripts` where your CoffeeScript tests go into. You define the Rails 3.1
asset pipeline manifest in `spec/javascripts/spec.js.coffee`:

    #=require_tree ./

### Rails 2 & 3

With Rails 3 you write your Jasmine specs in JavaScript, configured and server with the Jasmine gem. Please read the
detailed installation and configuration instructions at [the Jasmine Gem][].

### PhantomJS

You need the PhantomJS browser installed on your system. You can download binaries for Mac OS X and Windows from
[the PhantomJS download section][].

Alternatively you can install [Homebrew][] on Mac OS X and install it with:

    $ brew install phantomjs

If you are using Ubuntu 10.10, you can install it with apt:

    $ sudo add-apt-repository ppa:jerome-etienne/neoip
    $ sudo apt-get update
    $ sudo apt-get install phantomjs

You can also build it from source for several other operating systems, please consult the
[PhantomJS build instructions][].

## Usage

Please read the [Guard usage documentation](https://github.com/guard/guard#readme).

## Guardfile

Guard::Jasmine can be adapted to all kind of projects. Please read the
[Guard documentation](https://github.com/guard/guard#readme) for more information about the Guardfile DSL.

### Rails 3.1 with Jasminerice

    guard 'jasmine' do
      watch(%r{app/assets/javascripts/(.+)\.(js\.coffee|js)}) { |m| "spec/javascripts/#{m[1]}_spec.#{m[2]}" }
      watch(%r{spec/javascripts/(.+)_spec\.(js\.coffee|js)})  { |m| "spec/javascripts/#{m[1]}_spec.#{m[2]}" }
      watch(%r{spec/javascripts/spec\.(js\.coffee|js)})       { "spec/javascripts" }
    end

### Rails 2 & 3 with the Jasmine gem

    guard 'jasmine', :url => 'http://127.0.0.1:8888' do
      watch(%r{public/javascripts/(.+)\.js})                  { |m| "spec/javascripts/#{m[1]}_spec.js" }
      watch(%r{spec/javascripts/(.+)_spec\.js})               { |m| "spec/javascripts/#{m[1]}_spec.js" }
      watch(%r{spec/javascripts/support/jasmine\.yml})        { "spec/javascripts" }
      watch(%r{spec/javascripts/support/jasmine_config\.rb})  { "spec/javascripts" }
    end

## Options

There following options can be passed to Guard::Jasmine:


    :jasmine_url => 'http://192.168.1.5/jasmine'  # URL where Jasmine is served.
                                                  # default: http://127.0.0.1/jasmine

    :phantomjs_bin => '~/bin/phantomjs'           # Path to phantomjs.
                                                  # default: '/usr/local/bin/phantomjs'

    :all_on_start => false                        # Run all specs on start.
                                                  # default: true

    :keep_failed => false                         # Keep failed specs and add them the next run again.
                                                  # default: true

    :all_after_pass => false                      # Run all specs after a single spec has passed.
                                                  # default: true

    :notifications => false                       # Show success and error messages.
                                                  # default: true

    :hide_success => true                         # Disable successful compilation messages.
                                                  # default: false


## Alternatives

* [guard-jasmine-headless-webkit][], a Guard for [jasmine-headless-webkit][], but doesn't run on JRuby.
* [Evergreen][], runs CoffeeScript specs headless, but has no
continuous testing support.
* [Jezebel][] a Node.js REPL and continuous test runner for [Jessie][], a Node runner for Jasmine, but has no full
featured browser environment.

## Development

- Documentation hosted at [RubyDoc](http://rubydoc.info/gems/guard-jasmine/file/README.md).
- Source hosted at [GitHub](https://github.com/netzpirat/guard-Jasmine).
- Report issues and feature requests to [GitHub Issues](https://github.com/netzpirat/guard-Jasmine/issues).

Pull requests are very welcome! Please try to follow these simple "rules", though:

- Please create a topic branch for every separate change you make.
- Make sure your patches are well tested.
- Update the README (if applicable).
- Please **do not change** the version number.

For questions please join us on our [Google group](http://groups.google.com/group/guard-dev) or on `#guard`
(irc.freenode.net).

## Acknowledgment

- [Ariya Hidayat][] for [PhantomJS][], a powerful headless WebKit browser.
- [Brad Phelan][] for [Jasminerice][], an elegant solution for [Jasmine][] in the Rails 3.1 asset pipeline.
- [Pivotal Labs][] for their beautiful [Jasmine][] BDD testing framework that makes JavaScript testing fun.
- [Jeremy Ashkenas][] for [CoffeeScript][], that little language that compiles into JavaScript and makes me enjoy the
front-end.
- The [Guard Team][] for giving us such a nice piece of software that is so easy to extend, one *has* to make a plugin
for it!
- All the authors of the numerous [Guards][] available for making the Guard ecosystem so much growing and comprehensive.

## License

(The MIT License)

Copyright (c) 2011 Michael Kessler

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

[Guard]: https://github.com/guard/guard
[Guards]: https://github.com/guard
[Guard Team]: https://github.com/guard/guard/contributors
[Ariya Hidayat]: http://twitter.com/#!/AriyaHidayat
[PhantomJS]: http://www.phantomjs.org/
[the PhantomJS download section]: http://code.google.com/p/phantomjs/downloads/list
[PhantomJS build instructions]: http://code.google.com/p/phantomjs/wiki/BuildInstructions
[Brad Phelan]: http://twitter.com/#!/bradgonesurfing
[Jasminerice]: https://github.com/bradphelan/jasminerice
[Pivotal Labs]: http://pivotallabs.com/
[Jasmine]: http://pivotal.github.com/jasmine/
[the Jasmine Gem]: https://github.com/pivotal/jasmine-gem
[Jeremy Ashkenas]: http://twitter.com/#!/jashkenas
[CoffeeScript]: http://jashkenas.github.com/coffee-script/
[Rails 3.1 asset pipeline]: http://guides.rubyonrails.org/asset_pipeline.html
[Homebrew]: http://mxcl.github.com/homebrew/
[Jezebel]: https://github.com/benrady/jezebel
[Jessie]: https://github.com/futuresimple/jessie
[guard-jasmine-headless-webkit]: https://github.com/johnbintz/guard-jasmine-headless-webkit
[jasmine-headless-webkit]: https://github.com/johnbintz/jasmine-headless-webkit/
[Evergreen]: https://github.com/jnicklas/evergreen
