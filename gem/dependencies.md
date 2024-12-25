A gem dependencies file allows installation of a consistent set of gems across
multiple environments.  The RubyGems implementation is designed to be
compatible with Bundler's Gemfile format.  You can see additional
documentation on the format at:

  http://bundler.io

RubyGems automatically looks for these gem dependencies files:

* gem.deps.rb
* Gemfile
* Isolate

These files are looked up automatically using `gem install -g`, or you can
specify a custom file.

When the RUBYGEMS_GEMDEPS environment variable is set to a gem dependencies
file the gems from that file will be activated at startup time.  Set it to a
specific filename or to "-" to have RubyGems automatically discover the gem
dependencies file by walking up from the current directory.

You can also activate gem dependencies at program startup using
Gem.use_gemdeps.

NOTE: Enabling automatic discovery on multiuser systems can lead to execution
of arbitrary code when used from directories outside your control.

Gem Dependencies
================

Use #gem to declare which gems you directly depend upon:

  gem 'rake'

To depend on a specific set of versions:

  gem 'rake', '~> 10.3', '>= 10.3.2'

RubyGems will require the gem name when activating the gem using
the RUBYGEMS_GEMDEPS environment variable or Gem::use_gemdeps.  Use the
require: option to override this behavior if the gem does not have a file of
that name or you don't want to require those files:

  gem 'my_gem', require: 'other_file'

To prevent RubyGems from requiring any files use:

  gem 'my_gem', require: false

To load dependencies from a .gemspec file:

  gemspec

RubyGems looks for the first .gemspec file in the current directory.  To
override this use the name: option:

  gemspec name: 'specific_gem'

To look in a different directory use the path: option:

  gemspec name: 'specific_gem', path: 'gemspecs'

To depend on a gem unpacked into a local directory:

  gem 'modified_gem', path: 'vendor/modified_gem'

To depend on a gem from git:

  gem 'private_gem', git: 'git@my.company.example:private_gem.git'

To depend on a gem from github:

  gem 'private_gem', github: 'my_company/private_gem'

To depend on a gem from a github gist:

  gem 'bang', gist: '1232884'

Git, github and gist support the ref:, branch: and tag: options to specify a
commit reference or hash, branch or tag respectively to use for the gem.

Setting the submodules: option to true for git, github and gist dependencies
causes fetching of submodules when fetching the repository.

You can depend on multiple gems from a single repository with the git method:

  git 'https://github.com/rails/rails.git' do
    gem 'activesupport'
    gem 'activerecord'
  end

Gem Sources
===========

RubyGems uses the default sources for regular `gem install` for gem
dependencies files.  Unlike bundler, you do need to specify a source.

You can override the sources used for downloading gems with:

  source 'https://gem_server.example'

You may specify multiple sources.  Unlike bundler the prepend: option is not
supported. Sources are used in-order, to prepend a source place it at the
front of the list.

Gem Platform
============

You can restrict gem dependencies to specific platforms with the #platform
and #platforms methods:

  platform :ruby_21 do
    gem 'debugger'
  end

See the bundler Gemfile manual page for a list of platforms supported in a gem
dependencies file.:

  http://bundler.io/v1.6/man/gemfile.5.html

Ruby Version and Engine Dependency
==================================

You can specify the version, engine and engine version of ruby to use with
your gem dependencies file.  If you are not running the specified version
RubyGems will raise an exception.

To depend on a specific version of ruby:

  ruby '2.1.2'

To depend on a specific ruby engine:

  ruby '1.9.3', engine: 'jruby'

To depend on a specific ruby engine version:

  ruby '1.9.3', engine: 'jruby', engine_version: '1.7.11'

Grouping Dependencies
=====================

Gem dependencies may be placed in groups that can be excluded from install.
Dependencies required for development or testing of your code may be excluded
when installed in a production environment.

A #gem dependency may be placed in a group using the group: option:

  gem 'minitest', group: :test

To install dependencies from a gemfile without specific groups use the
`--without` option for `gem install -g`:

  $ gem install -g --without test

The group: option also accepts multiple groups if the gem fits in multiple
categories.

Multiple groups may be excluded during install by comma-separating the groups for `--without` or by specifying `--without` multiple times.

The #group method can also be used to place gems in groups:

  group :test do
    gem 'minitest'
    gem 'minitest-emoji'
  end

The #group method allows multiple groups.

The #gemspec development dependencies are placed in the :development group by
default.  This may be overridden with the :development_group option:

  gemspec development_group: :other

