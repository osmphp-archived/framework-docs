# OsmShop: Browser Auto-Completion

For every input browser tries to suggest values entered before. How to make the best auto-completion experience? 

It turns out, auto-complete behavior is described in detail in the [Living HTML5 Standard](https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#autofilling-form-controls:-the-autocomplete-attribute).

It is also nicely summed up in [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete).

Several things to remember:

1. Tell the browser **not to remember** (and later offer) entered certain information. It includes sensitive information (passwords, health details or other secrets) or information that is unique by nature (product SKU, title or URL key):

        autocomplete="off"

    In Osm framework, use `autocomplete` property of form control:
    
        'title' => StringField::new([
            ...
            'autocomplete' => 'off',
        ]),

2. Hint the browser if the input collects **standard user details**:

        // full name
        autocomplete="name"
        
        // country code
        autocomplete="country"
        
        // country name
        autocomplete="country-name"

        // For shipping and billing addresses add "shipping" and "billing" prefixes:
        autocomplete="billing country-name"

        // Rare: for relative persons, use "section-" prefix:
        autocomplete="section-spouse billing country-name"

    In Osm framework, use `autocomplete` property of form control:
    
        'title' => StringField::new([
            ...
            'autocomplete' => 'country',
        ]),
        
3. Use **standard element names** as browsers [use some heuristics](https://source.chromium.org/chromium/chromium/src/+/master:components/autofill/core/common/autofill_regex_constants.cc?originalUrl=https:%2F%2Fcs.chromium.org%2Fchromium%2Fsrc%2Fcomponents%2Fautofill%2Fcore%2Fcommon%2Fautofill_regex_constants.cc) to recognize what a field is about:

        // search field
        name="q"
    
4. Otherwise, prefix element name with area and CRUD names to limit auto-complete scope only to a specific part of OsmShop (but **submit the field without the prefix**):

        // limit auto-completion scope to backend-products CRUD 
        name="backend-products__title"  
        
    In Osm framework, use form's `autocomplete_prefix` property:

        'form' => Form::new([
            ...
            'autocomplete_prefix' => 'backend-products__',
        ]),
        