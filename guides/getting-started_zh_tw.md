[Permalink](http://susy.oddbird.net/guides/ "Permalink to Susy: Getting Started")

# Susy: 入門

## [安裝][10]

安裝有以下四種方法：

- 使用命令列安裝到 Compass 環境或現有專案
- 安裝到 Rails 專案
- 手動安裝
- 手動安裝到 fire.app

### 使用命令列安裝到 [Compass][11] 環境

從命令列安裝：

    # 命令列
    gem install susy


建立一個新的 [Compass][12] 專案：

    # 命令列
    compass create  -r susy -u susy


或安裝到現有的 [Compass][12] 專案：

    # config.rb
    require "susy"


### 安裝到 [Rails 3.x][13] 專案

    # Gemfile
    gem "susy"


你可能還需要：

    # Gemfile
    gem 'compass', '&gt;= 0.12.2'
    gem 'compass-rails', '&gt;= 1.0.3'


之後執行：

    # command line
    bundle install


### [手動安裝][14]

You can use this method if you're not using Compass from Terminal and/or Rails. This is going to work with CodeKit.

*   Simply [download][15] the zip file from GitHub
*   Copy the contents of the "sass" folder *feel free to remove everything else
*   Paste the files in your projects "sass" folder (or whatever you call it)
*   And import Susy! ( See [Usage][16] ) And you're good to go!

### 安裝到 fire.app

    待補步驟

## [Usage][16]

    @import "susy";


### [Settings][17]

Set up your default grid values: total columns, column width, and gutter width.

    $total-columns  : 12;             // a 12-column grid
    $column-width   : 4em;            // each column is 4em wide
    $gutter-width   : 1em;            // 1em gutters between columns
    $grid-padding   : $gutter-width;  // grid-padding equal to gutters


### [Basic Grids][18]

The basic Susy grid is composed using two simple mixins:

*   Use the [container()][19] mixin to create your initial grid context.
*   Use the [span-columns()][20] mixin to declare the width of an element on the grid.

Here's a simple page layout:

    .page {
      // page acts as a container for our grid.
      @include container;

      // header and footer are full-width by default.
      header, footer { clear: both; }

      // nav spans 3 columns of total 12.
      nav { @include span-columns(3,12); }

      .content {
        // content spans the final (omega) 9 columns of 12.
        @include span-columns(9 omega,12);

        // main content spans 6 of those 9 columns.
        .main { @include span-columns(6,9); }

        // secondary content spans the final 3 (omega) of 9 columns.
        .secondary { @include span-columns(3 omega,9); }
      }
    }


### [Responsive Grids][21]

Responsive Susy grids allow you to change the number of columns in a layout at different window sizes, using @media-queries with min and max widths. This requires one more mixin:

*   Use [at-breakpoint()][22] to set different layouts at min- and max-width breakpoints.

Here's a mobile-first example:

    $total-columns: 4;

    .page {
      // Establish our default 4-column grid container.
      @include container;

      // Create a media-query breakpoint at a min-width of 30em
      // And use a larger 8-column grid within that media-query.
      @include at-breakpoint(30em 8) {
        // Establish our new 8-column container.
        @include container;
      }
    }


### [Advanced][23]

Susy is built to handle your unique markup, and any number of edge cases. It includes the standard [push()][24] and [pull()][25] mixins, along with other useful functions and shortcuts, support for various grid styles, and even bi-directional grids for multi-lingual sites. Check the [reference documentation][26] for details.

## [Troubleshooting][27]

### [Version Management][28]

When you are working with bundled gems and dependencies across a number of different projects, managing gem versions can become an issue.

If you are working in a **Ruby** environment, we recommend using [RVM][29]. See our [Rails troubleshooting][30] below for some basic instructions, or [dig into RVM's installation instructions][29].

In a **Python** environment, we recommend [virtualenv][31] in conjunction with these ["postactivate" and "predeactivate" scripts][32] to add support for Ruby gems.

Once you have that in place, [Bundler][33] can be used in either environment to manage the actual installation and updating of the gems.

### [Compass Install][34]

The old gem and the new gem have different names, but are required simply as `susy`. That can cause a conflict if both gems are present.

If you have installed Susy in the past, make sure you've uninstalled older versions:

    # command line
    gem uninstall compass-susy-plugin
    # "compass-susy-plugin" was the gem name for 0.9.x and lower
    # Susy 1.0 switches to "susy" as the gem name


And then install 1.0:

    # command line
    gem install susy


Then use Compass as normal.

### [Rails 3.x Install][30]

We recommend you use [RVM][35] for using Susy with Rails projects. It has become the standard gem management system for Rails, it's very easy to install and use, and it helps create and manage Gemsets among different developers working on different branches.

[Here are some RVM best practices][36]:

If you have installed Susy in the past, make sure you've uninstalled older versions. See [Compass Install][34] above.

[Install RVM][29] (These are basics, if you do not have Ruby and Rails already installed in your environment, we [recommend you use RVM's installation instructions][29]):

    # command line
    # from your system's root:
    curl -L get.rvm.io | bash -s stable


Create a gemset for your site:

    # command line
    rvm gemset create fooBar


Create an `.rvmrc` file at your site's root:

    # .rvmrc
    rvm use 1.9.3@fooBar
    # Use whatever Ruby version number your app uses


Now whenever you `cd` into your site's root, RVM will pick up and use that Gemset.

`cd` to your site and install [Bundler][33]:

    # command line
    gem install bundler


Add Susy to your `Gemfile` ([more info on Gemfiles][37]):

    gem "susy", "~&gt; 1.0.5"


And finally run your bundle:

    # command line
    bundle


 [1]: http://susy.oddbird.net/guides/
 [2]: http://www.compass-style.org/
 [3]: http://susy.oddbird.net/guides/getting-started/
 [4]: http://susy.oddbird.net/guides/reference/
 [5]: http://susy.oddbird.net/guides/demos/
 [6]: http://susy.oddbird.net/guides/sites-using-susy/
 [7]: https://github.com/ericam/susy
 [8]: http://stackoverflow.com/questions/tagged/susy-compass
 [9]: http://twitter.com/compasssusy/
 [10]: http://susy.oddbird.net/guides/#start-install
 [11]: http://susy.oddbird.net/guides/#start-compass
 [12]: http://compass-style.org/
 [13]: http://susy.oddbird.net/guides/#start-rails
 [14]: http://susy.oddbird.net/guides/#start-manual
 [15]: https://github.com/ericam/susy/archive/master.zip
 [16]: http://susy.oddbird.net/guides/#start-usage
 [17]: http://susy.oddbird.net/guides/#start-settings
 [18]: http://susy.oddbird.net/guides/#start-basic
 [19]: http://susy.oddbird.net/guides/reference/#ref-container
 [20]: http://susy.oddbird.net/guides/reference/#ref-span-columns
 [21]: http://susy.oddbird.net/guides/#start-responsive
 [22]: http://susy.oddbird.net/guides/reference/#ref-at-breakpoint
 [23]: http://susy.oddbird.net/guides/#start-advanced
 [24]: http://susy.oddbird.net/guides/reference/#ref-push
 [25]: http://susy.oddbird.net/guides/reference/#ref-pull
 [26]: http://susy.oddbird.net/guides/reference/
 [27]: http://susy.oddbird.net/guides/#troubleshooting
 [28]: http://susy.oddbird.net/guides/#troubleshooting-versions
 [29]: http://rvm.io/rvm/install/
 [30]: http://susy.oddbird.net/guides/#troubleshooting-rails-install
 [31]: http://www.virtualenv.org/en/latest/index.html
 [32]: https://gist.github.com/1078601
 [33]: http://gembundler.com/
 [34]: http://susy.oddbird.net/guides/#troubleshooting-compass-install
 [35]: http://rvm.io
 [36]: http://rvm.io/rvm/best-practices/
 [37]: http://gembundler.com/gemfile.html
