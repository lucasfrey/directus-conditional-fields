# Directus conditional fields

Conditional fields is a custom extension that allow you to hide and show fields in a collection called `block` (for now).
In order to work, you will need to create a collection with these parameters:

### Type dropdown
The `type` dropdown will host the event listener that will trigger the hide and show fields

### Fields naming convention
Once you have setup your type field, you need to name your fields like the above

*NOTE:* currently the collection fields need to be named `block`, but we could easily update that in the future

| collection | type      | field   |
|------------|-----------|---------|
| block      | editorial | text    |
| block      | editorial | intro   |
| block      | image     | picture |
| block      | image     | alt     |

With that config, if you select the type `editorial` in the dropdown, only the `text` and `intro` fields will appear on the screen.

### Import conditional-fields into the collection
Once you have setup your fields, you can then just add the conditional-fields field so the javascript can do his job on the administration page.


## Usage
If you update this module, don't forget to run the production build like this :

```
npm run build
```

And to drop all the files from `directus-conditional-fields/dist` into `public/extensions/custom/interfaces/directus-conditional-fields`

Happy to hear for improvement, use the issues traker to open anything you want.
