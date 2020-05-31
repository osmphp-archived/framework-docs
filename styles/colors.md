# Colors

{{ toc }}

## Usage

Specify view's background color using `on_color` property and view's foreground color using `color` property:

    'form' => Form::new([
        ...
        'on_color' => 'primary',
        'color' => 'neutral',
    ]),

If omitted, colors are inherited from the parent view.

### Background Colors

Theme's background colors are defined in the `$bg-colors` global SASS variable. Standard `Osm_Blank` theme introduces the following colors:

* `neutral` - default (white) background
* `primary`, `secondary`, `primary-dark`, `primary-light`, `secondary-dark`, `secondary-light` - **brand colors** - derived from site owners' brand logo 
* `success`, `warning`, `error`, `danger` - **emotion colors** - used to express alert level or state
* `space` - used around page dialog views, for instance, on the login page.
* `message` - neutral message color, used in snack bars

### Foreground Colors

Theme's foreground colors are defined in the `$fg-colors` global SASS variable. Standard `Osm_Blank` theme introduces the following colors:

* `neutral` - default foreground color - black on light backgrounds and white on dark backgrounds.
* `primary`, `secondary`, `primary-dark`, `primary-light`, `secondary-dark`, `secondary-light` - **brand colors** - same as in the background color list. Don't use brand color on other brand or emotion background. If light brand color is displayed on light background, SASS logic may pick darker variation, and vice versa. 
* `success`, `warning`, `error`, `danger` - **emotion colors** - same as in the background color list, may differ for the light and dark backgrounds. Don't use emotion color on other emotion background.
* `disabled` - used to indicate that an element is disabled, may differ for the light and dark backgrounds.

## Blade Templates

Render background and foreground colors in the Blade template of the view using computed `on_color_` and `color_` properties:

    <button ...class="button {{ $view->on_color_ }} {{ $view->color_ }}" ...

They are rendered as `-on-{color}` and`-{color}` CSS modifiers, respectively.

## SCSS Files

Background and foreground colors are automatically applied based on `-on-{color}` and`-{color}` CSS modifiers. However, sometimes additional styling is needed.

### `currentColor`

Standard `currentColor` CSS constant is handy to apply element's `color` not only to text, but also to borders:
 
    .button {
        borderColor: currentColor;  
    }

### Computed Foreground Colors 

Use the following SASS functions to render computed foreground colors:

* `fg-delimiter-color($bg)` - used to render delimiter lines on `$bg` background. 
* `fg-highlighted-color($bg, $fg)` - used to highlight an element displayed using `$fg` color on `$bg` background 
* `fg-pressed-color($bg, $fg)` - used to indicate an element displayed using `$fg` color on `$bg` background is "pressed" or clicked.

### `foreach-background` Mixin

Use `foreach-background` mixin to apply every possible background color to the element:

    .menu-bar {
      .-delimiter, .-delimiter-copy {
        @include foreach-background() using($bg) {
          border-color: fg-delimiter-color($bg);
        }
      }
    }

**Note**. Make sure that CSS "block" class (in the example above, `.menu-bar`) is the first in a selector that includes a `foreach-background` mixin. 

For instance, `&__item` is translated into `.menu-bar__item` - not a "block" class. Instead, use `& &__item` selector which translates into `.menu-bar .menu-bar__item`.

### `foreach-foreground` Mixin

Use `foreach-background` mixin to apply every possible foreground color to the element:

    .button {
      &:active {
        @include foreach-background() using($bg) {
          @include foreach-foreground($bg) using($bg, $fg) {
            background-color: fg-pressed-color($bg, $fg);
          }
        }
      }
    }

**Note**. Make sure that CSS "block" class (in the example above, `.menu-bar`) is the first in a selector that includes a `foreach-foreground` mixin. 
