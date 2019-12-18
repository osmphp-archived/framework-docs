# Mustache Templates

Osm framework does all the rendering on the server using layers and Blade templates. In some cases, however, it is beneficial to render HTML in JavaScript. To do that, use [Mustache](https://mustache.github.io/) or any other Javascript templating library.

Contents:

{{ toc }}

## Basic Usage

1. Define a Mustache template in any Blade template in a `<script>` element with `text/template` type:

        <script type="text/template" id="my-template">
            <div>@{{ placeholder }}</div>
            <script>
                alert('hi!');
            <@/script>
        </script> 

    **Note**. `@` in `@{{ placeholder }}` tells Blade not to process the placeholder on the server, but to render `{{ placeholder }}` as is. This placeholder will be processed by Mustache in JavaScript.

    **Note**. Use `<@/script>` to close `<script>` elements which are part of your Mustache template. Use `</script>` to close `<script>` element containing the template.   

2. Use the template in JavaScript by the ID of `<script>` element:

        import templates from 'Osm_Framework_Js/vars/templates';
        import Mustache from 'mustache';
        import macaw from "Osm_Framework_Js/vars/macaw";
        
        // ...
        
        templates.get('my-template').then(html => {
            target.innerHTML = Mustache.render(html, {
                placeholder: 'Hello, world!'
            });

            // in case your Mustache template adds view models to
            // the newly rendered HTML elements, attach controllers
            // to the new view models 
            macaw.afterInserted(element);
        });

## Delayed Template Loading

The example above renders Mustache templates into the page source. It is even better to load Mustache templates after the page is loaded using AJAX:  

1. In shell, create a new Mustache template which will handle dynamic loading of a template:

        osm create:mustache my-ajax-template
        
2. Edit generated Blade template `{module_path}/{area}/views/my-ajax-template.blade.php` rendering the Mustache template:

        <div>@{{ placeholder }}</div>
        <script>
            alert('hi!');
        </script>

    **Note**. AJAX Mustache template are not wrapped into `<script>` element and their `</script>` elements don't have to be escaped with `@`.
    
3. Use the template in JavaScript by the ID you used in `templates.add()` call:

        import templates from 'Osm_Framework_Js/vars/templates';
        import Mustache from 'mustache';
        import macaw from "Osm_Framework_Js/vars/macaw";
        
        // ...
        
        templates.get('my-ajax-template').then(html => {
            target.innerHTML = Mustache.render(html, {
                placeholder: 'Hello, world!'
            });

            // in case your Mustache template adds view models to
            // the newly rendered HTML elements, attach controllers
            // to the new view models 
            macaw.afterInserted(element);
        });
            