## REFERENCES

http://www.intridea.com/blog/2012/3/8/polishing-rubies-part-i
http://www.intridea.com/blog/2012/3/15/polishing-rubies-part-ii

## PATH WHERE I HAVE WORKED: 

  /Users/casiano/Dropbox/src/ruby/building_gems/bleigh_tutorial_to_building_gems/my-gem

## STEPS

## 1 *

      $ bundle gem my-gem
        create  my-gem/Gemfile
        create  my-gem/Rakefile
        create  my-gem/LICENSE.txt
        create  my-gem/README.md
        create  my-gem/.gitignore
        create  my-gem/my-gem.gemspec
        create  my-gem/lib/my-gem.rb
        create  my-gem/lib/my-gem/version.rb
        Initializating git repo in /Users/casiano/Dropbox/src/ruby/building_gems/bleigh_tutorial_to_building_gems/my-gem


## 2 *
      $ cd my-gem/
      $ ls -la
      total 48
      drwxrwxr-x  10 casiano  staff   340 18 oct 16:12 .
      drwxrwxr-x   6 casiano  staff   204 18 oct 16:13 ..
      drwxrwxr-x  10 casiano  staff   340 18 oct 16:12 .git
      -rw-rw-r--   1 casiano  staff   154 18 oct 16:12 .gitignore
      -rw-rw-r--   1 casiano  staff    91 18 oct 16:12 Gemfile
      -rw-rw-r--   1 casiano  staff  1078 18 oct 16:12 LICENSE.txt
      -rw-rw-r--   1 casiano  staff   495 18 oct 16:12 README.md
      -rw-rw-r--   1 casiano  staff    28 18 oct 16:12 Rakefile
      drwxrwxr-x   4 casiano  staff   136 18 oct 16:12 lib
      -rw-rw-r--   1 casiano  staff   717 18 oct 16:12 my-gem.gemspec

## 3 *
        $ git status -s
        A  .gitignore
        A  Gemfile
        A  LICENSE.txt
        A  README.md
        A  Rakefile
        A  lib/my-gem.rb
        A  lib/my-gem/version.rb
        A  my-gem.gemspec

## 4 *
        $ git commit -m 'vreated structure with "bundle gem my-gem"'
        [master (root-commit) e438015] vreated structure with "bundle gem my-gem"
         8 files changed, 104 insertions(+)
         create mode 100644 .gitignore
         create mode 100644 Gemfile
         create mode 100644 LICENSE.txt
         create mode 100644 README.md
         create mode 100644 Rakefile
         create mode 100644 lib/my-gem.rb
         create mode 100644 lib/my-gem/version.rb
         create mode 100644 my-gem.gemspec
        $ git status -s
        $ 

## 5 * Sign up in rubygems.org and then sign-in 

## 6 * Use gem yank to yank (remove) old gems 

           $ gem yank hola_casiano -v 0.0.1

## 7 * 
         $ cat -n my-gem.gemspec 
         1  # -*- encoding: utf-8 -*-
         2  lib = File.expand_path('../lib', __FILE__)
         3  $LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
         4  require 'my-gem/version'
         5  
         6  Gem::Specification.new do |gem|
         7    gem.name          = "my-gem"
         8    gem.version       = My::Gem::VERSION
         9    gem.authors       = ["Casiano Rodriguez Leon"]
        10    gem.email         = ["casiano.rodriguez.leon@gmail.com"]
        11    gem.description   = %q{TODO: Write a gem description}
        12    gem.summary       = %q{TODO: Write a gem summary}
        13    gem.homepage      = ""
        14  
        15    gem.files         = `git ls-files`.split($/)
        16    gem.executables   = gem.files.grep(%r{^bin/}).map{ |f| File.basename(f) }
        17    gem.test_files    = gem.files.grep(%r{^(test|spec|features)/})
        18    gem.require_paths = ["lib"]
        19  end

## 8 * 
    The last step of our initial setup is to ensure that the constant
    and folder names generated by Bundler matches the constant and
    folder names we want to use for our library. 
    In the above gemspec the referenced version is My::Gem::VERSION when I would
    actually want it to be MyGem::VERSION. To change this, I need to
    modify the constant in three places:

            The version reference in the gemspec
            The lib/my-gem.rb file
            The lib/my-gem/version.rb file

    Unless our library is going to be very small, we will usually
    want our base constant (for example MyGem) to be a module, not a
    class. This allows us to easily namespace all of the different
    classes and components of our gem in a simple and sane way.

## 9 * 
   gem names are almost always delimited with dashes 
   while folder and file names are almost always delimited with underscores. 


   This is partially to make for inferrable mappings from file names to class
   names.
   So if I have a constant called MyGem::VERSION it should be located in 
                      lib/my_gem/version.rb
   even though my gems name is my-gem.

## 10 * 

    When Bundler generates our gem (if we have dashes in the name) 
    it creates a folder inside the lib directory of the same name. I
    would recommend modifying this to use underscores in place of dashes.
    To do this, we will need to do the following:

      Rename the directory (e.g. mv lib/my-gem lib/my_gem).
      Alter the first line require in lib/my-gem.rb to use the
        underscored directory name.
      Alter the require in the gemspec file to use the underscored
        directory name.

        $ cd lib
        $ git mv my-gem my_gem
        $ vi my-gem.rb 
        $ git diff my-gem.rb
        diff --git a/lib/my-gem.rb b/lib/my-gem.rb
        index b3ecddc..7deb65a 100644
        --- a/lib/my-gem.rb
        +++ b/lib/my-gem.rb
        @@ -1,4 +1,4 @@
        -require "my-gem/version"
        +require "my_gem/version"
         
         module My
           module Gem
        $ cd ..
        $ vi my-gem.gemspec 
        $ git diff my-gem.gemspec
        diff --git a/my-gem.gemspec b/my-gem.gemspec
        index 4a98e0d..4d53936 100644
        --- a/my-gem.gemspec
        +++ b/my-gem.gemspec
        @@ -1,7 +1,7 @@
         # -*- encoding: utf-8 -*-
         lib = File.expand_path('../lib', __FILE__)
         $LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
        -require 'my-gem/version'
        +require 'my_gem/version'
         
        $ git status -s
         M README
         M lib/my-gem.rb
        R  lib/my-gem/version.rb -> lib/my_gem/version.rb
         M my-gem.gemspec

         Gem::Specification.new do |gem|
           gem.name          = "my-gem"



       Also:

               $ cat lib/my-gem.rb 
               require "my_gem/version"

               module MyGem
                   # Your code goes here...
               end

           $ cat lib/my_gem/version.rb 
           module MyGem
             VERSION = "0.0.1"
           end

## 11 * By following the community conventions you:

      Make your library easier to "guess" how to use because
      developers can use common idioms to which they have become
      accustomed.

      Make your library auto-requirable by Bundler (which looks
      for the my-gem.rb file and requires it automatically in
      frameworks such as Rails)

      Make your library easier for contributors to join by providing
      familiar patterns and locations for code

## 12 * When you package up your new rubygem to share with the world, you
    need to be careful with the files you place directly in lib/.
    Rubygems (and almost all other ruby package mangers) will add your
    gem’s lib/ to the load path. This means any file placed in the top
    level of lib/ will be directly requirable by anyone using the gem.

    Bad gem example:

      `-- lib
          |-- foo
          |   `-- cgi.rb
          |-- foo.rb
          |-- erb.rb
          `-- set.rb

    It may seem harmless to call files whatever you’d like in your
    package because you are “namespaced” in your own package. But if
    lib/ is prepended to $LOAD_PATH it will clobber Ruby’s built in erb
    and set libs. require 'erb' would no longer require Ruby’s builtin
    erb library, but this package’s version of it.

    The safe (and correct) way would be to namespace your files under
    another directory. Its conventional to create a folder within lib
    with the same name as your gem. Then we would put all our dependency
    files under lib/foo/ instead of at lib/ root.

    This is sort of a gray area. There is no strict rule that you must
    put all your files under a folder with your package name. It is
    okay to have multiple files at your root lib directory as long as
    you intend for people to require them separately. Namespace internal
    dependency files that you don’t expect for people to require directly.

## 13 * There are two types of gem dependencies, 
    runtime dependencies and development dependencies. 
    
    Runtime dependencies are libraries that
    your gem needs in order to function. 

    Development dependencies, on the other hand, are gems that are only 
    needed by people who are going to contribute to the code of the gem. 

    Gems are, by default, installed only with the runtime dependencies. 

## 14 * Semantic Versioning (http://semver.org/)

     A normal version number MUST take the form X.Y.Z where X, Y, and Z
     are non-negative integers. 
     X is the major version, 
     Y is the minor version, and 
     Z is the patch version. 
     Each element MUST increase numerically by increments of one. 
     For instance: 1.9.0 -> 1.10.0 -> 1.11.0.

     '~>' is called "spermy operator"
     "~> 2.3" means "equal to 2.3 or greater than 2.3, but less than 3.0", 
     while "~> 2.3.0" would mean "equal to 2.3.0 or greater than 2.3.0, 
     but less than 2.4.0".

## 15 * The first step to using RSpec in your tests is to add it to our project's 
    gemspec:

             gem.add_development_dependency 'rspec', '~>2.11'

    This tells RubyGems that RSpec is a necessary library for developers
    who are going to try to contribute to your gem. Once you've added
    the dependency, run bundle to update your dependencies with Bundler.
    Note that because Bundler is capable of reading dependencies from
    a gemspec you don't need to manually specify them in your Gemfile.

     $ bundle install
     Fetching gem metadata from https://rubygems.org/.
     Error Bundler::HTTPError during request to dependency API
     Fetching full source index from https://rubygems.org/
     Using rake (0.9.2.2) 
     Using i18n (0.6.1) 
     ...
     Installing rails (3.2.8) 
     Using my-gem (0.0.1) from source at 
       /Users/casiano/Dropbox/src/ruby/building_gems/bleigh_tutorial_to_building_gems/my-gem 
     Using rspec-core (2.11.1) 
     Using rspec-expectations (2.11.3) 
     Installing rspec-mocks (2.11.3) 
     Using rspec (2.11.0) 
     Your bundle is complete! 
     Use `bundle show [gemname]` to see where a bundled gem is installed.

## 16 * With RSpec, it's a good practice to specify the most recent minor version 

    (the second number in the MAJOR.MINOR.PATCH versioning system) for
    your dependency. This way if RSpec updates to a new major version
    that breaks existing functionality, you can review any breakages
    and fix them with a new release of your gem rather than having
    unstable tests in the wild.

## 17 * run:
       $ bundle exec rspec --init
       The --configure option no longer needs any arguments, so true was ignored.
         create   spec/spec_helper.rb
         create   .rspec
       $ cat .rspec 
       --color
       --format progress

       $ tree
       .
       ├── Gemfile
       ├── Gemfile.lock
       ├── LICENSE.txt
       ├── README
       ├── README.md
       ├── Rakefile
       ├── lib
       │   ├── my-gem.rb
       │   └── my_gem
       │       └── version.rb
       ├── my-gem.gemspec
       └── spec
           └── spec_helper.rb

## 18 * If you've written tests before, you should be familiar with
     spec/spec_helper.rb as a central place to bootstrap your test suite
     and add any dependent library requires, etc.

     $ cat spec/spec_helper.rb 
     # This file was generated by the `rspec --init` command. Conventionally, all
     # specs live under a `spec` directory, which RSpec adds to the `$LOAD_PATH`.
     # Require this file using `require "spec_helper"` to ensure that it is only
     # loaded once.
     #
     # See http://rubydoc.info/gems/rspec-core/RSpec/Core/Configuration
     RSpec.configure do |config|
       config.treat_symbols_as_metadata_keys_with_true_values = true
       config.run_all_when_everything_filtered = true
       config.filter_run :focus

       # Run specs in random order to surface order dependencies. If you find an
       # order dependency and want to debug it, you can fix the order by providing
       # the seed, which is printed after each run.
       #     --seed 1234
       config.order = 'random'
     end

## 19 * Now that we have our spec_helper all set up, let's go ahead and
     create our first spec. Create a file spec/my_gem_spec.rb (replace
     my_gem with your gem's name) and put this in it:

     require 'spec_helper'
     require 'my-gem'

     describe MyGem do
       it 'requires additional testing'
     end

## 20 * 
        $ cat Rakefile
        $:.unshift File.dirname(__FILE__) + 'lib'
        require "bundler/gem_tasks"

        require 'rspec/core/rake_task'
        RSpec::Core::RakeTask.new
        task :default => :spec

## 21 *

        $ rake -T
        rake build    # Build my-gem-0.0.1.gem into the pkg directory
        rake install  # Build and install my-gem-0.0.1.gem into system gems
        rake release  # Create tag v0.0.1 and build and push my-gem-0.0.1.gem to Rubygems
        rake spec     # Run RSpec code examples

## 22 *
        $ rake
        /usr/local/Cellar/ruby/1.9.3-p125/bin/ruby -S rspec ./spec/my_gem_spec.rb
        Run options: include {:focus=>true}

        All examples were filtered out; ignoring {:focus=>true}
        *

        Pending:
          MyGem requires additional testing
            # Not yet implemented
            # ./spec/my_gem_spec.rb:5

        Finished in 0.00023 seconds
        1 example, 0 failures, 1 pending

        Randomized with seed 30724

## 23 * Installing guard:

        $ cat Gemfile
        source 'https://rubygems.org'

        # Specify your gem's dependencies in my-gem.gemspec
        gemspec
        gem 'guard'
        gem 'guard-rspec'
        gem 'guard-bundler'

## 24 * Run bundle

## 25 * Guard works similarly to Bundler and Rake by creating a Guardfile 
     in your project's root directory. These commands automatically add
     example configuration for each guard type to the Guardfile (after
     guard init creates the file to begin with). 

          $ cat Guardfile 
          guard 'bundler' do
            watch('Gemfile')
            watch(/^.+\.gemspec/)
          end

          guard 'rspec', :version => 2 do
            watch(%r{^spec/.+_spec\.rb$})
            watch(%r{^lib/(.+)\.rb$})     { |m| "spec/#{m[1]}_spec.rb" }
            watch('spec/spec_helper.rb')  { "spec" }
          end

## 26 * Guard works by watching certain files in your project and then
     performing actions when those files change. The first guard tells
     us to watch for the Gemfile and the gemspec to change, and re-bundle
     the application when that happens. The second guard tells us to
     watch all of our RSpec test files, our spec_helper, and the
     corresponding library files for each of our RSpec tests. This means
     that when you change a file, Guard can automatically re-run the
     tests for that specific file only.

     Now that your Guardfile is properly configured, you can just run
                     bundle exec guard 
     from your project's root directory. This will get
     Guard up and running and you will now automatically run tests and
     re-bundle as you build your gem. Running Guard makes the development
     feedback loop as tight and automatic as possible.

    $ bundle exec guard
    Guard could not detect any of the supported notification libraries.
    Guard is now watching at 
      '/Users/casiano/Dropbox/src/ruby/building_gems/blei.../my-gem'
    Bundle already up-to-date
    Guard::RSpec is running, with RSpec 2!
    Running all specs
    Run options: include {:focus=>true}

    All examples were filtered out; ignoring {:focus=>true}
    *

    Pending:
      MyGem requires additional testing
        # Not yet implemented
        # ./spec/my_gem_spec.rb:5

    Finished in 0.00032 seconds
    1 example, 0 failures, 1 pending

    Randomized with seed 58712

    [Listen warning]:
      Listen will be polling changes. 
      Learn more at https://github.com/guard/listen#polling-fallback.

    Running: spec/my_gem_spec.rb
    Run options: include {:focus=>true}

    All examples were filtered out; ignoring {:focus=>true}
    *

## 27 * Continuous Integration

     Continuous Integration is the process of automatically running
     the tests for a project when new code is pushed to the central
     repository. Luckily, a team of developers has built Travis CI
     that provides free continuous integration for open source
     projects of all types.
     Travis allows you to test your code across multiple versions 
     of Ruby. 


     a) Go to http://travis-ci.org and sign in with your GitHub account.
     Travis needs access to your GitHub info to be able to set up the
     automatic build process for each repository.

     b) Hover over your name once logged in and go to your profile. This
     lists out your open source repositories; from here you can simply
     "switch on" CI for each project.

     c) Set up a .travis.yml file in your project.


