
[Permalink](http://susy.oddbird.net/guides/reference/ "Permalink to Susy: Reference")

# Susy: Reference

## [Basic Usage][10]

    @import 'susy';

*   **Container**: The root element of a *Grid*.
*   **Layout**: The total number of *Columns* in a grid.
*   **Grid Padding**: Padding on the sides of the *Grid*.
*   **Context**: The number of *Columns* spanned by the parent element.
*   **Omega**: Any *Grid Element* spanning the last *Column* in its *Context*.

### [Basic Settings][11]

#### [Total Columns][12]

The number of Columns in your default Grid Layout.

    // $total-columns: ;
    $total-columns: 12;

*   `<number>`: Unitless number.

#### [Column Width][13]

The width of a single Column in your Grid.

    // $column-width: ;
    $column-width: 4em;

*   `<length>`: Length in any unit of measurement (em, px, %, etc).

#### [Gutter Width][14]

The space between Columns.

    // $gutter-width: ;
    $gutter-width: 1em;

*   ``: Units must match `$column-width`.

#### [Grid Padding][15]

Padding on the left and right of a Grid Container.

    // $grid-padding: ;
    $grid-padding: $gutter-width;  // 1em

*   ``: Units should match the container width (`$column-width` unless `$container-width` is set directly).

### [Basic Mixins][16]

#### [Container][17]

Establish the outer grid-containing element.

    // container([$]*)
    .page { @include container; }

*   ``: Optional media-layout shortcuts (see '[Responsive Grids][18]' below). Default: `$total-columns`.

#### [Span Columns][19]

Align an element to the Susy Grid.

    // span-columns( [ , , , ])
    nav { @include span-columns(3,12); }
    article { @include span-columns(9 omega,12); }

*   ``: The number of *Columns* to span.
    *   ``: Optional flag to signal the last element in a row.
*   ``: Current nesting *Context*. Default: `$total-columns`.
*   ``: Optional padding applied inside an individual grid element. Given as a length (same units as the grid) or a list of lengths (from-direction to-direction). Default: `false`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Omega][20]

Apply to any omega element as an override.

    // omega([])
    .gallery-image {
      @include span-columns(3,9); // each gallery-image is 3 of 9 cols.
      &amp;:nth-child(3n) { @include omega; } // every third image completes a row.
    }

*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Nth-Omega][20]

Apply to any element as an nth-child omega shortcut. Defaults to `:last-child`.

    // nth-omega([, , ])
    .gallery-image {
      @include span-columns(3,9); // each gallery-image is 3 of 9 cols.
      @include nth-omega(3n); // same as omega example above.
    }

*   ``: The keyword or equation to select: `[first | only | last | ]`. An equation could be e.g. `3` or `3n` or `'3n%2B1'`. Note that quotes are needed to keep complex equations from being simplified by Compass. Default: `last`.
*   ``: The type of element, and direction to count from: `[child | last-child | of-type | last-of-type ]`. Default: `child`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

## [Responsive Grids][18]

*   **Breakpoint**: A min- or max- viewport width at which to change *Layouts*.
*   **Media-Layout**: Shortcut for declaring *Breakpoints* and *Layouts* in Susy.

### [Media-Layouts][21]

    // $media-layout:    ;
    // - You must supply either  or .
    $media-layout: 12;          // Use 12-col layout at matching min-width.
    $media-layout: 30em;        // At min 30em, use closest fitting layout.
    $media-layout: 30em 12;     // At min 30em, use 12-col layout.
    $media-layout: 12 60em;     // Use 12 cols up to max 60em.
    $media-layout: 30em 60em;   // Between min 30em &amp; max 60em, use closest layout.
    $media-layout: 30em 12 60em;// Use 12 cols between min 30em &amp; max 60em.
    $media-layout: 60em 12 30em;// Same. Larger length will always be max-width.
    $media-layout : 12 lt-ie9;  // Output is included under `.lt-ie9` class,
                                // for use with IE conditional comments
                                // on the  tag.

*   ``: Any length with units, used to set media breakpoints.
*   ``: Any (unitless) number of columns to use for the grid at a given breakpoint.
*   ``: Any string to use as a fallback class when mediaqueries are not available. Do not include a leading "`.`" class-signifier, simply the class name ("`lt-ie9`", not "`.lt-ie9`"). This can be anything you want: "`no-mediaqueries`", "`ie8`", "`popcorn`", etc.

### [Responsive Mixins][22]

#### [At-Breakpoint][23]

At a given min- or max-width Breakpoint, use a given Layout.

    // at-breakpoint( [, ]) {  }
    @include at-breakpoint(30em 12) {
      .page { @include container; }
    }

*   ``: The *Breakpoint/Layout* combo to use (see above).
*   ``: Browsers interpret em-based media-queries using the browser default font size (`16px` in most cases). If you have a different base font size for your site, we have to adjust for the difference. Tell us your base font size, and we'll do the conversion. Default: `$base-font-size`.
*   ``: Nested `@content` block will use the given *Layout*.

#### [Layout][24]

Set an arbitrary Layout to use with any block of content.

    // layout() {  }
    @include layout(6) {
      .narrow-page { @include container; }
    }

*   ``: The number of *Columns* to use in the *Layout*.
*   ``: Nested `@content` block will use the given *Layout*.

#### [Set Container Width][25]

Reset the width of a Container for a new Layout context. Can be used when `container()` has already been applied to an element, for DRYer output than simply using `container` again.

    // set-container-width([])
    @include container;
    @include at-breakpoint(8) {
      @include set-container-width;
    }

*   ``: The number of *Columns* to be contained. Default: Current value of `$total-columns` depending on *Layout*.

#### [With Grid Settings][26]

Use different grid settings for a block of code - whether the same grid at a different breakpoint, or a different grid altogether.

    // with-grid-settings([, , , ]) {  }
    @include with-grid-settings(12,4em,1.5em,1em) {
      .new-grid { @include container; }
    };

*   ``: Overrides the `$total-columns` setting for all contained elements.
*   ``: Overrides the `$column-width` setting for all contained elements.
*   ``: Overrides the `$gutter-width` setting for all contained elements.
*   ``: Overrides the `$grid-padding` setting for all contained elements.
*   ``: Nested `@content` block will use the given grid settings.

## [Grid Helpers][27]

### [Box Sizing][28]

#### [Border-Box Sizing][29]

Set the default box-model to `border-box`, and adjust the grid math accordingly.

    // border-box-sizing()
    @include border-box-sizing;

This will apply border-box model to all elements (using the star selector) and set `$border-box-sizing` to `true`. You can use the variable on it's own to adjust the grid math, in cases where you want to apply the box-model separately.

### [Padding Mixins][30]

#### [Prefix][31]

Add Columns of empty space as `padding` before an element.

    // prefix( [, , ])
    .box { @include prefix(3); }

*   ``: The number of *Columns* to be added as `padding` before.
*   ``: The *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Suffix][32]

Add columns of empty space as padding after an element.

    // suffix( [, , ])
    .box { @include suffix(2); }

*   ``: The number of *Columns* to be added as `padding` after.
*   ``: The *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Pad][33]

Shortcut for adding both Prefix and Suffix `padding`.

    // pad([, , , ])
    .box { @include pad(3,2); }

*   ``: The number of *Columns* to be added as `padding` before.
*   ``: The number of *Columns* to be added as `padding` after.
*   ``: The *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

### [Margin Mixins][34]

#### [Pre][35]

Add columns of empty space as margin before an element.

    // pre( [, , ])
    .box { @include pre(2); }

*   ``: The number of *Columns* to be added as `margin` before.
*   ``: The *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Post][36]

Add columns of empty space as margin after an element.

    // post( [, , ])
    .box { @include post(3); }

*   ``: The number of *Columns* to be added as `margin` after.
*   ``: The *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Squish][37]

Shortcut to add empty space as margin before and after an element.

    // squish([, , , ])
    .box { @include squish(2,3); }

*   ``: The number of *Columns* to be added as `margin` before.
*   ``: The number of *Columns* to be added as `margin` after.
*   ``: The *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Push][38]

Identical to [pre][35].

    // push( [, , ])
    .box { @include push(3); }

#### [Pull][39]

Add negative margins before an element, to pull it against the flow.

    // pull( [, , ])
    .box { @include pull(2); }

*   ``: The number of *Columns* to be subtracted as `margin` before.
*   ``: The *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

### [Reset Mixins][40]

#### [Reset Columns][41]

Resets an element to default block behaviour.

    // reset-columns([])
    article { @include span-columns(6); }     // articles are 6 cols wide
    #news article { @include reset-columns; } // but news span the full width
                                              // of their container

*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Remove-Omega][42]

Apply to any previously-omega element to reset it's float direction and margins to match non-omega grid elements. Note that unlike omega, this requires a context when nested.

    // remove-omega([, ])
    .gallery-image {
      &amp;:nth-child(3n) { @include remove-omega; } // 3rd images no longer complete rows.
    }

*   ``: Current nesting *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

#### [Remove Nth-Omega][43]

Apply to any previously nth-omega element to reset it's float direction and margins to match non-omega grid elements. Note that unlike omega, this requires a context when nested.

    // remove-nth-omega([, , , ])
    .gallery-image {
      @include remove-nth-omega(3n); // same as remove-omega example above.
    }

*   ``: The keyword or equation to select: `[first | only | last | ]`. An equation could be e.g. `3` or `3n` or `'3n%2B1'`. Note that quotes are needed to keep a complex equation from being simplified by Compass. Default: `last`.
*   ``: The type of element, and direction to count from: `[child | last-child | of-type | last-of-type ]`. Default: `child`.
*   ``: Current nesting *Context*. Default: `$total-columns`.
*   ``: The origin direction of your document flow. Default: `$from-direction`.

### [Other Mixins][44]

#### [Susy Grid Background][45]

Show the Susy Grid as a background-image on any container.

    // susy-grid-background();
    .page { @include susy-grid-background; }

*   If you are using the `` element as your *Container*, you need to apply a background to the `` element in order for this grid-background to size properly.
*   Some browsers have trouble with sub-pixel rounding on background images. Use this for checking general spacing, not pixel-exact alignment. Susy columns tend to be more accurate than gradient grid-backgrounds.

### [Functions][46]

Where a mixin returns property/value pairs, functions return simple values that you can put where you want, and use for advanced math.

#### [Columns][47]

Similar to [span-columns][19] mixin, but returns the math-ready `%` multiplier.

    // columns( [, ])
    .item { width: columns(3,6); }

*   ``: The number of *Columns* to span,
*   ``: The *Context*. Default: `$total-columns`.

#### [Gutter][48]

The `%` width of one gutter in any given context.

    // gutter([])
    .item { margin-right: gutter(6) %2B columns(3,6); }

*   ``: The *Context*. Default: `$total-columns`.

#### [Space][49]

Total `%` space taken by Columns, including internal AND external gutters.

    // space( [, ])
    .item { margin-right: space(3,6); }

*   ``: The number of *Columns* to span,
*   ``: The *Context*. Default: `$total-columns`.

### [Container Override Settings][50]

#### [Container Width][25]

Override the total width of your grid with an arbitrary length.

    // $container-width:  | ;
    $container-width: false;

*   ``: Length in em, px, %, etc.
*   ``: True or false.

#### [Container Style][51]

Override the type of shell containing your grid.

    // $container-style: ;
    $container-style: magic;

*   ``: `magic` | `static` | `fluid`.
    *   `magic`: Susy's magic grid has a set width, but becomes fluid rather than overflowing the viewport at small sizes.
    *   `static`: Susy's static grid will retain the width defined in your settings at all times.
    *   `fluid`: Susy's fluid grid will always be based on the viewport width. The percentage will be determined by your grid settings, or by `$container-width`, if either is set using `%` units. Otherwise it will default to `auto` (100%).

### [Direction Override Settings][52]

#### [From Direction][53]

The side of the Susy Grid from which the flow starts. For ltr documents, this is the left.

    // $from-direction: ;
    $from-direction: left;

*   ``: `left` | `right`

#### [Omega Float][54]

The direction that Omega elements should be floated.

    // $omega-float: ;
    $omega-float: opposite-position($from-direction);

*   ``: `left` | `right`

### [Compass Options][55]

#### [Base Font Size][56]

From the [Compass Vertical Rhythm][57] module, Susy uses your base font size to help manage em-based media-queries.

    // $base-font-size: ;
    $base-font-size: 16px;

*   ``: Any length in `px`. This will not actually effect your font size unless you use other Vertical Rhythm tools, we just need to know. See [Compass Docs][58] for further usage details.

#### [Browser Support][59]

Susy recognizes all the [Compass Browser Support][60] variables, although only IE6 and IE7 have special cases attached to them currently.

    // $legacy-support-for-ie  : ;
    // $legacy-support-for-ie6 : ;
    // $legacy-support-for-ie7 : ;
    $legacy-support-for-ie  : true;
    $legacy-support-for-ie6 : $legacy-support-for-ie;
    $legacy-support-for-ie7 : $legacy-support-for-ie;

*   ``: `true` | `false`

### [Breakpoint Output][55]

If you are compiling seperate files for IE-fallbacks, it can be useful to output only the modern code in one file and only the fallbacks in another file. You can make `at-breakpoint` do exactly that by using the following settings.

#### [$breakpoint-media-output][61]

Turn off media-query output for IE-only stylesheets.

    // $breakpoint-media-output: ;
    $breakpoint-media-output: true;

*   ``: `true` | `false`

#### [$breakpoint-ie-output][62]

Turn off media-query fallback output for non-IE stylesheets.

    // $breakpoint-ie-output: ;
    $breakpoint-ie-output: true;

*   ``: `true` | `false`

#### [$breakpoint-raw-output][63]

Pass through raw output without media-queries or fallback classes for IE-only stylesheets.

    // $breakpoint-raw-output: ;
    $breakpoint-raw-output: false;

*   ``: `true` | `false`

 [1]: http://susy.oddbird.net/
 [2]: http://www.compass-style.org/
 [3]: http://susy.oddbird.net/guides/getting-started/
 [4]: http://susy.oddbird.net/guides/reference/
 [5]: http://susy.oddbird.net/demos/
 [6]: http://susy.oddbird.net/sites-using-susy/
 [7]: https://github.com/ericam/susy
 [8]: http://stackoverflow.com/questions/tagged/susy-compass
 [9]: http://twitter.com/compasssusy/
 [10]: http://susy.oddbird.net/reference/#ref-basic
 [11]: http://susy.oddbird.net/reference/#ref-basic-settings
 [12]: http://susy.oddbird.net/reference/#ref-total-columns
 [13]: http://susy.oddbird.net/reference/#ref-column-width
 [14]: http://susy.oddbird.net/reference/#ref-gutter-width
 [15]: http://susy.oddbird.net/reference/#ref-grid-padding
 [16]: http://susy.oddbird.net/reference/#ref-basic-mixins
 [17]: http://susy.oddbird.net/reference/#ref-container
 [18]: http://susy.oddbird.net/reference/#ref-responsive
 [19]: http://susy.oddbird.net/reference/#ref-span-columns
 [20]: http://susy.oddbird.net/reference/#ref-omega
 [21]: http://susy.oddbird.net/reference/#ref-media-layouts
 [22]: http://susy.oddbird.net/reference/#ref-responsive-mixins
 [23]: http://susy.oddbird.net/reference/#ref-at-breakpoint
 [24]: http://susy.oddbird.net/reference/#ref-layout
 [25]: http://susy.oddbird.net/reference/#ref-container-width
 [26]: http://susy.oddbird.net/reference/#ref-with-settings
 [27]: http://susy.oddbird.net/reference/#ref-helper
 [28]: http://susy.oddbird.net/reference/#ref-helper-sizing
 [29]: http://susy.oddbird.net/reference/#ref-border-box-mixin
 [30]: http://susy.oddbird.net/reference/#ref-helper-padding
 [31]: http://susy.oddbird.net/reference/#ref-prefix
 [32]: http://susy.oddbird.net/reference/#ref-suffix
 [33]: http://susy.oddbird.net/reference/#ref-pad
 [34]: http://susy.oddbird.net/reference/#ref-helper-margin
 [35]: http://susy.oddbird.net/reference/#ref-pre
 [36]: http://susy.oddbird.net/reference/#ref-post
 [37]: http://susy.oddbird.net/reference/#ref-squish
 [38]: http://susy.oddbird.net/reference/#ref-push
 [39]: http://susy.oddbird.net/reference/#ref-pull
 [40]: http://susy.oddbird.net/reference/#ref-helper-reset
 [41]: http://susy.oddbird.net/reference/#ref-reset-columns
 [42]: http://susy.oddbird.net/reference/#ref-remove-omega
 [43]: http://susy.oddbird.net/reference/#ref-remove-nth-omega
 [44]: http://susy.oddbird.net/reference/#ref-helper-other
 [45]: http://susy.oddbird.net/reference/#ref-grid-background
 [46]: http://susy.oddbird.net/reference/#ref-helper-functions
 [47]: http://susy.oddbird.net/reference/#ref-columns
 [48]: http://susy.oddbird.net/reference/#ref-gutter
 [49]: http://susy.oddbird.net/reference/#ref-space
 [50]: http://susy.oddbird.net/reference/#ref-container-override
 [51]: http://susy.oddbird.net/reference/#ref-container-style
 [52]: http://susy.oddbird.net/reference/#ref-direction-override
 [53]: http://susy.oddbird.net/reference/#ref-from-direction
 [54]: http://susy.oddbird.net/reference/#ref-omega-float
 [55]: http://susy.oddbird.net/reference/#ref-compass-options
 [56]: http://susy.oddbird.net/reference/#ref-base-font-size
 [57]: http://compass-style.org/reference/compass/typography/vertical_rhythm/
 [58]: http://compass-style.org/reference/compass/typography/vertical_rhythm/#const-base-font-size
 [59]: http://susy.oddbird.net/reference/#ref-browser-support
 [60]: http://compass-style.org/reference/compass/support/
 [61]: http://susy.oddbird.net/reference/#ref-media-output
 [62]: http://susy.oddbird.net/reference/#ref-ie-output
 [63]: http://susy.oddbird.net/reference/#ref-raw-output
