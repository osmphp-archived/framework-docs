# Generating Mobile Menu Items For A Menu Bar

{{ toc }}

## The Problem

Menu bar's popup menu has the same items as menu bar itself. They are similar in many ways: same behavior (command, link or checkbox), same checked state, deeper submenus are reused.

However, not everything is the same. For instance, a popup menu item is **logically **visible if its matching menu bar item is **logically** visible and vice versa, but actually visible is only one of them, depending on menu bar's width.

Also in terms of events, menu bar should catch popup menu's `menuitem` events and fire them as its own.  

## Attempt No. 1

I just reused menu bar items in the popup menu:

    <nav class="menu-bar {{$view->modifier}}" id="{{$view->id_}}">
        <ul class="menu-bar__items">
            @foreach ($view->items_ as $child)
                @if (!$child->empty)
                    <li class="menu-bar__item {{ $child->type }}" id="{{$child->id_}}">
                        @include ($child->menu_bar_template, ['view' => $child])
                    </li>
                    @if ($child->menu_bar_view_model)
                        {!! $child->menu_bar_view_model_script !!}
                    @endif
                    <?php
                        /** @noinspection PhpUndefinedVariableInspection */
                        unset($child->id_);
                    ?>
                @endif
            @endforeach
        </ul>
        <div class="menu-bar__show-more">
            @include($view->menu)
        </div>
    </nav>

And In `MenuBar` view:

    protected function default($property) {
        switch ($property) {
            case 'menu': return PopupMenu::new([
                'alias' => 'menu',
                'items' => $this->items,
            ]);
        }

        return parent::default($property);
    }

## Cloning

It is obvious that a view should only be rendered once. So I implemented cloning of all the menu items just before rendering, that is before their computed properties are populated:

    public function rendering() {
        $this->menu = PopupMenu::new([
            'alias' => 'menu',
            'items' => $this->cloneObjects($this->items),
        ]);

        parent::rendering();
    }

This object cloning algorithm is a candidate for making a reusable `Cloner` object. 
 

The problem was they don't seem to work well this way while rendering menu bar item HTML ID has been computed and while rendering the same view as popup menu item it was reused. Not good - 2 HTML elements with the same ID.

