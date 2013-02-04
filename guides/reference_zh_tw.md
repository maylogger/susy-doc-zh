[Permalink](http://susy.oddbird.net/reference/)

## <a href="#ref-basic" id="ref-basic">基本用法</a>

    :::scss
    @import 'susy';

#### 名詞解釋：

為了保有名詞跟程式的相關性，採用保留原文，附註括弧翻譯。

- **Container**（容器）: 指 _Grid（格線）_最外面的框框。
- **Layout**（布局）: 一個格線擁有的 _Columns（欄）_ 數量。
- **Grid Padding**（留邊）: _Grid（格線）_ 兩旁的留白空間。
- **Context**（上層元件欄數）: 上層元件被分派的 _Columns（欄）_ 數量。
- **Omega**: 所有 _Grid Element（格線元件）_ 在被分割 _Context（上層元件欄數）_ 中的最後一 _Column（欄）_。

### <a href="#ref-basic-settings" id="ref-basic-settings">基本設定</a>

#### <a href="#ref-total-columns" id="ref-total-columns">Total Columns 總欄數</a>

格線裡的總欄數

    :::scss
    // $total-columns: <number>;
    $total-columns: 12;
    // 設定此格線總共12欄

- `<number>`: 沒有單位的數字。

#### <a href="#ref-column-width" id="ref-column-width">Column Width 欄寬</a>

格線裡面一個欄位的寬度

    :::scss
    // $column-width: <length>;
    $column-width: 4em;
    // 設定一個欄 4em 寬

- `<length>`: 任何長度單位的尺寸 (em, px, %, etc)

#### <a href="#ref-gutter-width" id="ref-gutter-width">Gutter Width 欄間隔</a>

欄與欄的間隔

    :::scss
    // $gutter-width: <length>;
    $gutter-width: 1em;
    // 設定欄與欄有 1em 的間隔

- `<length>`: 單位必須與 `$column-width` 相同。

#### <a href="#ref-grid-padding" id="ref-grid-padding">Grid Padding 留邊</a>

格線左邊與右邊的留白

    :::scss
    // $grid-padding: <length>;
    $grid-padding: $gutter-width;  // 1em
    // 這邊是設定兩邊留白跟欄間寬度相同

- `<length>`: 單位必須與容器寬度相同（如果容器寬度有設定的話）
  (`$column-width（欄寬）` unless `$container-width（容器寬度）` is set directly).

### <a href="#ref-basic-mixins" id="ref-basic-mixins">基本 Mixins</a>

#### <a href="#ref-container" id="ref-container">Container（容器）</a>

建立最外面的格線容器元素

    :::scss
    // container([$<media-layout>]*)
    .page { @include container; }
    // 把 <div class="page"></div> 作為整個格線最外面的容器

- `<$media-layout>`: [選填] media-layout（媒介版型）快速設定
  （參考下面的 '[Responsive Grids（響應式格線）][responsive]' ）
  預設: `$total-columns`.

[responsive]: #ref-responsive

#### <a href="#ref-span-columns" id="ref-span-columns">Span Columns（分派欄位）</a>

指派一個 HTML 元素對齊到格線上

    :::scss
    // span-columns(<$columns> [<omega> , <$context>, <$padding>, <$from>])
    nav { @include span-columns(3,12); }
    // 設定 <nav> 這個元素寬為 12 欄格線中的 3 欄
    article { @include span-columns(9 omega,12); }
    // 設定 <article> 這個元素的寬為 12 欄格線中的 9 欄，並且為 omega（最後一欄）屬性


- `<$columns>`（欄）： 跨越的 _Columns（欄位）_ 數。
  - `<omega>`（最後一欄）： [選填] 標記為唯一的最後一欄。
- `<$context>`（上層元件欄數）： 目前所在巢狀格線的 _Context（上層元件欄數）_。
  預設為： `$total-columns`（總欄位數）
- `<$padding>`（留白）： [選填] 在這一個格線元素內的 padding（留白）空間。
  請設定一個長度 （單位跟格線相同）
  或設定多個長度 (from-direction（從起頭方向） to-direction（從收尾方向）)。
  預設： `false`（無）
- `<$from>`： 指定你文件流向的源頭方向
  預設： `$from-direction`（從起頭方向）

#### <a href="#ref-omega" id="ref-omega">Omega（最後一欄）</a>

表示這是最後一欄的元素

    :::scss
    // omega([<$from>])
    .gallery-image {
      @include span-columns(3,9); // 每幅 gallery-image 佔了 9 欄中的 3欄
      &:nth-child(3n) { @include omega; } // 並且每 3 個圖片就會變成一行
    }

- `<$from>`： 指定你文件流向的源頭方向
  預設： `$from-direction`（從起頭方向）

#### <a href="#ref-omega" id="ref-nth-omega">Nth-Omega（某倍數的最後一欄）</a>

快速指定某個倍數的元素是最後一欄
預設值是： `:last-child`（最後一個子元素）

    :::scss
    // nth-omega([<$n>, <$selector>, <$from>])
    .gallery-image {
      @include span-columns(3,9); // 每幅 gallery-image 佔了 9 欄中的 3欄
      @include nth-omega(3n); // 並且每 3 個圖片就會變成一行，跟上面那個 demo 一樣效果
    }

- `<$n>`: The keyword or equation to select: `[first | only | last | <equation>]`.
  An equation could be e.g. `3` or `3n` or `'3n+1'`.
  Note that quotes are needed to keep complex equations
  from being simplified by Compass.
  Default: `last`.
- `<$selector>`: The type of element, and direction to count from:
  `[child | last-child | of-type | last-of-type ]`.
  Default: `child`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

## <a href="#ref-responsive" id="ref-responsive">Responsive Grids</a>

- **Breakpoint**: A min- or max- viewport width at which to change _Layouts_.
- **Media-Layout**: Shortcut for declaring _Breakpoints_ and _Layouts_ in Susy.

### <a href="#ref-media-layouts" id="ref-media-layouts">Media-Layouts</a>

    :::scss
    // $media-layout: <min-width> <layout> <max-width> <ie-fallback>;
    // - You must supply either <min-width> or <layout>.
    $media-layout: 12;          // Use 12-col layout at matching min-width.
    $media-layout: 30em;        // At min 30em, use closest fitting layout.
    $media-layout: 30em 12;     // At min 30em, use 12-col layout.
    $media-layout: 12 60em;     // Use 12 cols up to max 60em.
    $media-layout: 30em 60em;   // Between min 30em & max 60em, use closest layout.
    $media-layout: 30em 12 60em;// Use 12 cols between min 30em & max 60em.
    $media-layout: 60em 12 30em;// Same. Larger length will always be max-width.
    $media-layout : 12 lt-ie9;  // Output is included under `.lt-ie9` class,
                                // for use with IE conditional comments
                                // on the <html> tag.

- `<$min/max-width>`: Any length with units, used to set media breakpoints.
- `<$layout>`: Any (unitless) number of columns to use for the grid
  at a given breakpoint.
- `<$ie-fallback>`: Any string to use as a fallback class
  when mediaqueries are not available.
  Do not include a leading "`.`" class-signifier,
  simply the class name ("`lt-ie9`", not "`.lt-ie9`").
  This can be anything you want:
  "`no-mediaqueries`", "`ie8`", "`popcorn`", etc.

### <a href="#ref-responsive-mixins" id="ref-responsive-mixins">Responsive Mixins</a>

#### <a href="#ref-at-breakpoint" id="ref-at-breakpoint">At-Breakpoint</a>
At a given min- or max-width Breakpoint, use a given Layout.

    :::scss
    // at-breakpoint(<$media-layout> [, <$font-size>]) { <@content> }
    @include at-breakpoint(30em 12) {
      .page { @include container; }
    }

- `<$media-layout>`: The _Breakpoint/Layout_ combo to use (see above).
- `<$font-size>`: Browsers interpret em-based media-queries
  using the browser default font size (`16px` in most cases).
  If you have a different base font size for your site,
  we have to adjust for the difference.
  Tell us your base font size, and we'll do the conversion.
  Default: `$base-font-size`.
- `<@content>`: Nested `@content` block will use the given _Layout_.

#### <a href="#ref-layout" id="ref-layout">Layout</a>
Set an arbitrary Layout to use with any block of content.

    :::scss
    // layout(<$layout-cols>) { <@content> }
    @include layout(6) {
      .narrow-page { @include container; }
    }

- `<$layout-cols>`: The number of _Columns_ to use in the _Layout_.
- `<@content>`: Nested `@content` block will use the given _Layout_.

#### <a href="#ref-container-width" id="ref-container-width">Set Container Width</a>
Reset the width of a Container for a new Layout context.
Can be used when `container()` has already been applied to an element,
for DRYer output than simply using `container` again.

    :::scss
    // set-container-width([<$columns>])
    @include container;
    @include at-breakpoint(8) {
      @include set-container-width;
    }

- `<$columns>`: The number of _Columns_ to be contained.
  Default: Current value of `$total-columns` depending on _Layout_.

#### <a href="#ref-with-settings" id="ref-with-settings">With Grid Settings</a>
Use different grid settings for a block of code -
whether the same grid at a different breakpoint,
or a different grid altogether.

    :::scss
    // with-grid-settings([<columns>, <width>, <gutter>, <padding>]) { <@content> }
    @include with-grid-settings(12,4em,1.5em,1em) {
      .new-grid { @include container; }
    };

- `<$columns>`: Overrides the `$total-columns` setting for all contained elements.
- `<$width>`: Overrides the `$column-width` setting for all contained elements.
- `<$gutter>`: Overrides the `$gutter-width` setting for all contained elements.
- `<$padding>`: Overrides the `$grid-padding` setting for all contained elements.
- `<@content>`: Nested `@content` block will use the given grid settings.

## <a href="#ref-helper" id="ref-helper">Grid Helpers</a>

### <a href="#ref-helper-sizing" id="ref-helper-sizing">Box Sizing</a>

#### <a href="#ref-border-box-mixin" id="ref-border-box-mixin">Border-Box Sizing</a>
Set the default box-model to `border-box`,
and adjust the grid math accordingly.

    :::scss
    // border-box-sizing()
    @include border-box-sizing;

This will apply border-box model to all elements
(using the star selector)
and set `$border-box-sizing` to `true`.
You can use the variable on it's own to adjust the grid math,
in cases where you want to apply the box-model separately.

### <a href="#ref-helper-padding" id="ref-helper-padding">Padding Mixins</a>

#### <a href="#ref-prefix" id="ref-prefix">Prefix</a>
Add Columns of empty space as `padding` before an element.

    :::scss
    // prefix(<$columns> [, <$context>, <$from>])
    .box { @include prefix(3); }

- `<$columns>`: The number of _Columns_ to be added as `padding` before.
- `<$context>`: The _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

#### <a href="#ref-suffix" id="ref-suffix">Suffix</a>
Add columns of empty space as padding after an element.

    :::scss
    // suffix(<$columns> [, <$context>, <$from>])
    .box { @include suffix(2); }

- `<$columns>`: The number of _Columns_ to be added as `padding` after.
- `<$context>`: The _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

#### <a href="#ref-pad" id="ref-pad">Pad</a>
Shortcut for adding both Prefix and Suffix `padding`.

    :::scss
    // pad([<$prefix>, <$suffix>, <$context>, <$from>])
    .box { @include pad(3,2); }

- `<$prefix>`: The number of _Columns_ to be added as `padding` before.
- `<$suffix>`: The number of _Columns_ to be added as `padding` after.
- `<$context>`: The _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

### <a href="#ref-helper-margin" id="ref-helper-margin">Margin Mixins</a>

#### <a href="#ref-pre" id="ref-pre">Pre</a>
Add columns of empty space as margin before an element.

    :::scss
    // pre(<$columns> [, <$context>, <$from>])
    .box { @include pre(2); }

- `<$columns>`: The number of _Columns_ to be added as `margin` before.
- `<$context>`: The _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

#### <a href="#ref-post" id="ref-post">Post</a>
Add columns of empty space as margin after an element.

    :::scss
    // post(<$columns> [, <$context>, <$from>])
    .box { @include post(3); }

- `<$columns>`: The number of _Columns_ to be added as `margin` after.
- `<$context>`: The _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

#### <a href="#ref-squish" id="ref-squish">Squish</a>
Shortcut to add empty space as margin before and after an element.

    :::scss
    // squish([<$pre>, <$post>, <$context>, <$from>])
    .box { @include squish(2,3); }

- `<$pre>`: The number of _Columns_ to be added as `margin` before.
- `<$post>`: The number of _Columns_ to be added as `margin` after.
- `<$context>`: The _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

#### <a href="#ref-push" id="ref-push">Push</a>
Identical to [pre](#ref-pre).

    :::scss
    // push(<$columns> [, <$context>, <$from>])
    .box { @include push(3); }

#### <a href="#ref-pull" id="ref-pull">Pull</a>
Add negative margins before an element, to pull it against the flow.

    :::scss
    // pull(<$columns> [, <$context>, <$from>])
    .box { @include pull(2); }

- `<$columns>`: The number of _Columns_ to be subtracted as `margin` before.
- `<$context>`: The _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

### <a href="#ref-helper-reset" id="ref-helper-reset">Reset Mixins</a>

#### <a href="#ref-reset-columns" id="ref-reset-columns">Reset Columns</a>
Resets an element to default block behaviour.

    :::scss
    // reset-columns([<$from>])
    article { @include span-columns(6); }     // articles are 6 cols wide
    #news article { @include reset-columns; } // but news span the full width
                                              // of their container

- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.


#### <a href="#ref-remove-omega" id="ref-remove-omega">Remove-Omega</a>
Apply to any previously-omega element
to reset it's float direction and margins
to match non-omega grid elements.
Note that unlike omega,
this requires a context when nested.

    :::scss
    // remove-omega([<$context>, <$from>])
    .gallery-image {
      &:nth-child(3n) { @include remove-omega; } // 3rd images no longer complete rows.
    }

- `<$context>`: Current nesting _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

#### <a href="#ref-remove-nth-omega" id="ref-remove-nth-omega">Remove Nth-Omega</a>
Apply to any previously nth-omega element
to reset it's float direction and margins
to match non-omega grid elements.
Note that unlike omega,
this requires a context when nested.

    :::scss
    // remove-nth-omega([<$n>, <$selector>, <$context>, <$from>])
    .gallery-image {
      @include remove-nth-omega(3n); // same as remove-omega example above.
    }

- `<$n>`: The keyword or equation to select: `[first | only | last | <equation>]`.
  An equation could be e.g. `3` or `3n` or `'3n+1'`.
  Note that quotes are needed to keep a complex equation from being simplified by Compass.
  Default: `last`.
- `<$selector>`: The type of element, and direction to count from:
  `[child | last-child | of-type | last-of-type ]`.
  Default: `child`.
- `<$context>`: Current nesting _Context_.
  Default: `$total-columns`.
- `<$from>`: The origin direction of your document flow.
  Default: `$from-direction`.

### <a href="#ref-helper-other" id="ref-helper-other">Other Mixins</a>

#### <a href="#ref-grid-background" id="ref-grid-background">Susy Grid Background</a>
Show the Susy Grid as a background-image on any container.

    :::scss
    // susy-grid-background();
    .page { @include susy-grid-background; }

- If you are using the `<body>` element as your _Container_,
  you need to apply a background to the `<html>` element
  in order for this grid-background to size properly.
- Some browsers have trouble with sub-pixel rounding on background images.
  Use this for checking general spacing, not pixel-exact alignment.
  Susy columns tend to be more accurate than gradient grid-backgrounds.

### <a href="#ref-helper-functions" id="ref-helper-functions">Functions</a>

Where a mixin returns property/value pairs, functions return simple values
that you can put where you want, and use for advanced math.

#### <a href="#ref-columns" id="ref-columns">Columns</a>
Similar to [span-columns](#ref-span-columns) mixin,
but returns the math-ready `%` multiplier.

    :::scss
    // columns(<$columns> [, <$context>])
    .item { width: columns(3,6); }

- `<$columns>`: The number of _Columns_ to span,
- `<$context>`: The _Context_.
  Default: `$total-columns`.

#### <a href="#ref-gutter" id="ref-gutter">Gutter</a>
The `%` width of one gutter in any given context.

    :::scss
    // gutter([<$context>])
    .item { margin-right: gutter(6) + columns(3,6); }

- `<$context>`: The _Context_.
  Default: `$total-columns`.

#### <a href="#ref-space" id="ref-space">Space</a>
Total `%` space taken by Columns, including internal AND external gutters.

    :::scss
    // space(<$columns> [, <$context>])
    .item { margin-right: space(3,6); }

- `<$columns>`: The number of _Columns_ to span,
- `<$context>`: The _Context_.
  Default: `$total-columns`.

### <a href="#ref-container-override" id="ref-container-override">Container Override Settings</a>

#### <a href="#ref-container-width" id="ref-container-width">Container Width</a>
Override the total width of your grid with an arbitrary length.

    :::scss
    // $container-width: <length> | <boolean>;
    $container-width: false;

- `<length>`: Length in em, px, %, etc.
- `<boolean>`: True or false.

#### <a href="#ref-container-style" id="ref-container-style">Container Style</a>
Override the type of shell containing your grid.

    :::scss
    // $container-style: <style>;
    $container-style: magic;

- `<style>`: `magic` | `static` | `fluid`.
  - `magic`: Susy's magic grid has a set width,
    but becomes fluid rather than overflowing the viewport at small sizes.
  - `static`: Susy's static grid will retain the width defined in your settings
    at all times.
  - `fluid`: Susy's fluid grid will always be based on the viewport width.
    The percentage will be determined by your grid settings,
    or by `$container-width`, if either is set using `%` units.
    Otherwise it will default to `auto` (100%).

### <a href="#ref-direction-override" id="ref-direction-override">Direction Override Settings</a>

#### <a href="#ref-from-direction" id="ref-from-direction">From Direction</a>
The side of the Susy Grid from which the flow starts.
For ltr documents, this is the left.

    :::scss
    // $from-direction: <direction>;
    $from-direction: left;

- `<direction>`: `left` | `right`

#### <a href="#ref-omega-float" id="ref-omega-float">Omega Float</a>
The direction that Omega elements should be floated.

    :::scss
    // $omega-float: <direction>;
    $omega-float: opposite-position($from-direction);

- `<direction>`: `left` | `right`

### <a href="#ref-compass-options" id="ref-compass-options">Compass Options</a>

#### <a href="#ref-base-font-size" id="ref-base-font-size">Base Font Size</a>
From the [Compass Vertical Rhythm][rhythm] module,
Susy uses your base font size to help manage
em-based media-queries.

    :::scss
    // $base-font-size: <px-size>;
    $base-font-size: 16px;

- `<px-size>`: Any length in `px`.
  This will not actually effect your font size
  unless you use other Vertical Rhythm tools,
  we just need to know.
  See [Compass Docs][base-font-size] for further usage details.

[rhythm]: http://compass-style.org/reference/compass/typography/vertical_rhythm/
[base-font-size]: http://compass-style.org/reference/compass/typography/vertical_rhythm/#const-base-font-size

#### <a href="#ref-browser-support" id="ref-browser-support">Browser Support</a>
Susy recognizes all the [Compass Browser Support][support] variables,
although only IE6 and IE7 have special cases attached to them currently.

    :::scss
    // $legacy-support-for-ie  : <boolean>;
    // $legacy-support-for-ie6 : <boolean>;
    // $legacy-support-for-ie7 : <boolean>;
    $legacy-support-for-ie  : true;
    $legacy-support-for-ie6 : $legacy-support-for-ie;
    $legacy-support-for-ie7 : $legacy-support-for-ie;

- `<boolean>`: `true` | `false`

[support]: http://compass-style.org/reference/compass/support/

### <a href="#ref-compass-options" id="ref-compass-options">Breakpoint Output</a>
If you are compiling seperate files for IE-fallbacks,
it can be useful to output only the modern code in one file
and only the fallbacks in another file.
You can make `at-breakpoint` do exactly that
by using the following settings.

#### <a href="#ref-media-output" id="ref-media-output">$breakpoint-media-output</a>
Turn off media-query output for IE-only stylesheets.

    :::scss
    // $breakpoint-media-output: <boolean>;
    $breakpoint-media-output: true;

- `<boolean>`: `true` | `false`

#### <a href="#ref-ie-output" id="ref-ie-output">$breakpoint-ie-output</a>
Turn off media-query fallback output for non-IE stylesheets.

    :::scss
    // $breakpoint-ie-output: <boolean>;
    $breakpoint-ie-output: true;

- `<boolean>`: `true` | `false`

#### <a href="#ref-raw-output" id="ref-raw-output">$breakpoint-raw-output</a>
Pass through raw output
without media-queries or fallback classes
for IE-only stylesheets.

    :::scss
    // $breakpoint-raw-output: <boolean>;
    $breakpoint-raw-output: false;

- `<boolean>`: `true` | `false`
