# Prefer Theme-Independent View Properties

{{ toc }}

## Problem

The latest fashion fo introducing `style` property (`Button`, menu `Item`) has the same problems as `modifier` property had. Not good.

As already suggested before, better properties for menu `Item`:

    'primary' => true, // bold 
    'important' => true, // italic 
    'dangerous' => true, // red 

And for `Button`:

    'primary' => true, // -filled 
    'important' => true, // -outlined 
    'dangerous' => true, // -error 

But how should then menu bar checkbox item be implemented instead of current:

    @include(Button::new([
        'alias' => 'button',
        'title' => $view->title,
        'style' => $view->checked
            ? $view->checked_button_style
            : $view->unchecked_button_style,
    ]))

It also operated button CSS classes in JS:

    set checked(value) {
        this.model.checked = value;

        if (value) {
            if (this.model.checked_button_style) {
                addClass(this.button_element, this.model.checked_button_style);
            }
            if (this.model.unchecked_button_style) {
                removeClass(this.button_element, this.model.unchecked_button_style);
            }
        }
        else {
            if (this.model.checked_button_style) {
                removeClass(this.button_element, this.model.checked_button_style);
            }
            if (this.model.unchecked_button_style) {
                addClass(this.button_element, this.model.unchecked_button_style);
            }
        }

        this.trigger('checked', {checked: value});
    }

If theme developer reimplements button (for instance, using Bootstrap CSS classes) she'll also have to modify checkbox menu item. 