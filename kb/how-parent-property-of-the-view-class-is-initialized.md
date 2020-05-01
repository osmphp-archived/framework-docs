# How `parent` Property Of The `View` Class Is Initialized

`parent` property of the `View` class is used while generating unique and human-friendly HTML element IDs. 

There 3 3 places in the framework responsible for automatically assigning the `parent` property:

{{ toc }}

## Layer Instruction's Direct Children

    '#page.items'  => [
        'surface' => MenuBar::new([...]),
    ],

The framework finds the view referenced by `#page`, merges the data into its properties, finally goes through the data's top level views and assigns the `#page` view as `parent`:

    // see Osm\Framework\Layers\Layout::merge()
    $view->assignSelfAsParentTo($value);


## View In A Blade Template

    @include(Button::new(['alias' => 'button', 'title' => osm_t("Normal Button")]))

The framework knows which view is currently being rendered and `Button` constructor assigns currently rendered view as its `parent`:

    // if no view is rendered, parent is set to null
    $this->parent = $this->rendering->current_view;

## Deeper Views

    '#page.items'  => [
        'surface' => MenuBar::new([
            'items' => [
                'file' => CommandItem::new([...]),
            ],
        ]),
    ],

PHP first creates `CommandItem` class instance, then `MenuBar` class instance. `MenuBar` constructor goes through its direct children and assigns itself as `parent`:

    $this->assignSelfAsParentTo($data);


