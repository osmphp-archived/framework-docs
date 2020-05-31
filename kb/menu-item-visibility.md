# Menu Item Visibility

{{ toc }}

## Properties

A menu item has a `hidden` property, both on in the server-side views and in the JS controllers. When it's `true`, menu item is not visible. When it's `false`, it ... depends.

Items in the popup menu of a menu bar are also hidden if the same item in the menu bar is visible by adding `-invisible` CSS modifier to them. 

## CSS Modifier

Both `-hidden` and `-invisible` CSS modifiers hide the menu item.

## Handling Delimiters

Menu delimiters are rendered as a `-delimiter` menu item modifier. 

When item visibility changes, `rearrangeDelimiters()` method is called which assigns `-delimiter-copy` CSS modifier to the first menu item in the delimited item group. This CSS modifier has the same style as `-delimiter`; however it is dynamic delimiter mimicking the real delimiter that can be hidden.