# Modal Dialogs

**Modal dialog** is a window, initially hidden, shown over the current page after you click a button or a menu command. "Modal" means that until dialog is closed you can't access anything else on the page.

Modal dialogs are displayed using [dynamically loaded Mustache templates](mustache-templates.html#dynamically-loaded-templates).

Most often, modal dialog displays a form.

Contents:

{{ toc }}

## Creating Modal Dialog

1. In shell, generate files which will define new modal dialog within the system and handle its dynamic loading:

        osm create:dialog hello
        
2. Open the dialog using `dialogs.show` method:

        import dialogs from "Osm_Ui_Dialogs/vars/dialogs";

        // ...
        
        dialogs.show('hello', {
            width: 500,
            height: 300,
        }).then(result => {
            if (!result) {
                // if the user canceled the modal dialog, do nothing            
                return;
            }

            // if user confirmed the action, process the dialog result
        }); 

## How Modal Dialog Is Loaded

1. Mustache template for dialog box starts loading just after browser displays a page, with the following line in `{module_path}/{area}/js/index.js`:

        templates.add('dialog__hello', {route: 'GET /dialogs/hello'});

2. The `GET /dialogs/hello` route is bound to a controller method in `{module_path}/config/{area}/routes.php`:

        'GET /dialogs/hello' => [
            'class' => Frontend::class,
            'method' => 'helloDialog',
            'public' => true,
        ],

3. The controller method, located in `{module_path}/Controllers/{Area}.php` file, renders dialog template from a layer:

        public function helloDialog() {
            return osm_layout('dialogs_hello');
        }

4. The layer, located in `{module_path}/{area}/layers/dialogs_hello.php` file, renders Mustache template for a popup window with a form and buttons in it:

        return [
            '@include' => ['modal_dialog'],
            '#dialog' => [
                'modifier' => '-hello',
                'header' => osm_t("{dialog_title}"),
                'views' => [
                    'form' => Form::new([
                        'route' => 'POST {form_route}',
                        'submitting_message' => osm_t("{submitting_message}"),
                        'views' => [
                            // add input fields here
                        ],
                    ]),
                ],
                'footer' => MenuBar::new([
                    'modifier' => '-center',
                    'items' => [
                        'ok' => [
                            'type' => Type::COMMAND,
                            'title' => osm_t("OK"),
                            'modifier' => '-filled',
                        ],
                        'cancel' => [
                            'type' => Type::COMMAND,
                            'title' => osm_t("Cancel"),
                        ],
                    ],
                ]),
            ],
        ];

## How Modal Dialog Works

1. As already mentioned, the dialog is shown using the following code:

        dialogs.show('hello', { width: 500, height: 300}).then(result => {
        }); 
    
2. Dialog dynamic behavior is handled by its JS controller class located in `{module_path}/{area}/js/HelloDialog.js` file:

        export default class HelloDialog extends CancelModalDialog {
            get events() {
                return Object.assign({}, super.events, {
                    'click &___footer__ok': 'onOk',
                    'form:success &__form': 'onFormSuccess',
                });
            }
        
            get form() {
                return macaw.get(document.getElementById(this.getAliasedId('&__form')), Form);
            }
        
            onOk() {
                this.form.submit();
            }
        
            onFormSuccess(e) {
                this.resolve(JSON.parse(e.detail));
            }
        };

3. Standard behavior:

    * cancel dialog if `Cancel` button or `Esc` is pressed;
    * submit form data via AJAX request if `OK` button is pressed;
    * pass form AJAX response into `result` parameter of `then()` callback.
 
4. Design form layout and handle form submission as described in [Forms](forms.html).