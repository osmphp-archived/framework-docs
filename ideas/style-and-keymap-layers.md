# Style And Keymap Layers

Themes can easily customize font, color and icon used in each element, in every page. 

Use CSS like rules in `{resource_path}/layers/{layer}__style.php`:

    return [
        '#header.color' => '-secondary',
        '#breadcrumb.color' => '-secondary-light',
        '#form.field_style' => '-filled',
        '#header.menu.items[content].items[products].icon' => '-products',
        '#title.menu.items[save].icon' => '-save',
    ];

Another ad-hoc layer, `{resource_path}/layers/{layer}__keymap.php`, may define key bindings.

    