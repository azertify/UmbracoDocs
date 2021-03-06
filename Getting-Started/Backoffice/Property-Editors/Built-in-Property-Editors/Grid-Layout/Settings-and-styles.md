#Settings and styling
A grid layout can also expose custom settings - such as data-attributes or styling options - on each cell or row. This allows editors to use a friendly UI to add configuration values to grid elements. When custom settings and styles are applied, they will by default be included in the grid html as either html attributes or inline styles.

![Grid layouts](images/settings.png)

These settings and styles must be configured by developers when setting up the grid layout data type.

###Configuring a custom setting or style
To add a setting, click the edit settings link. This will expand a dialog showing you the raw configuration data. This data is in the JSON format and will only save if its valid JSON.

The settings data could look like this, with an object for each setting:

    [
      {
        "label": "Class",
        "description": "Set a css class",
        "key": "class",
        "view": "textstring",
        "modifier": "col-sm-{0}",
        "applyTo": "row|cell"
      }
    ]

The different values are:

- **label** : Field name displayed in the content editor UI
- **description** : Descriptive text displayed in the content editor UI to guide the user
- **key** : The key the entered setting value will be stored under.
- **view** : The editor used to enter a setting value with.
- **prevalues** : For views that need predefined values, e.g. the radiobuttonlist view.
- **modifier (optional)** : A string formater to modify the output of the editor to prepend or append extra values.
- **applyTo (optional)** : States whether the setting can be used on a cell or a row. If either not present or empty, the setting will be shown both on Rows and Cells.

**label** and **description** are straight-forward.

**key** defines the alias the configuration is stored under and by default the alias of the attribute will also be the attribute on the rendered html element. In the example above any value entered in this settings editor will be rendered in the grid html as:

    <div **class**="VALUE-ENTERED"></div>

By changing the key of the setting you can modify the `<div>` element's attributes like `class`, `title`, `id` or custom `data-*` attributes.


**view** the view defines the editor used to enter a value. By default Umbraco comes with a collection of prevalue editors:

- textstring
- textarea
- radiobuttonlist
- mediapicker
- imagepicker
- boolean
- treepicker
- treesource
- number
- multivalues

Alternatively you can also pass in a path to a custom view like "/app_plugins/grid/editors/view.html"

**prevalues** is for views that need predefined values, e.g. the radiobuttonlist view. Prevalues are defined as strings in an array:
    
    "prevalues":[
        "value_1",
        "value_2",
        "value_3"
    ]

and will translate in to three different options where each string will become a radiobutton. The strings represent the value of the options.

**modifier** is a basic way to prepend, append or wrap the value from the editor in a simple string. This is especially useful when working with custom styles which often requires additional values to function. For instance if you want to set a background image you can get an image path from the image picker view. But in order for it to work with css it has to be wrapped in `url()`. In that case you set the **modifier** to `url('{0}')` which means that `{0}` is replaced with the editor value.


###Sample settings
There are many ways to combine these, here are some samples:

**Set a background image style**

    {
        "label": "Background image",
        "description": "Choose an image",
        "key": "background-image",
        "view": "imagepicker",
        "modifier": "url('{0}')"
    }


**Set a title setting**

    {
        "label": "Title",
        "description": "Set a title on this element",
        "key": "title",
        "view": "textstring"
    }


**Set a data-custom setting**

    {
        "label": "Custom data",
        "description": "Set the custom data on this element",
        "key": "data-custom",
        "view": "radiobuttonlist",
        "prevalues": [
            "value_1",
            "value_2",
            "value_3"
        ]
    }

###Multiple settings and styles
You can add multiple settings and styles configurations on a datatype. This is done by creating a new setting or style object. Remember to separate the objects with a comma.

**Adding multiple settings**

    [
        {
            "label": "Class",
            "description": "Set a class on this element",
            "key": "class",
            "view": "textstring"
        },
        {
            "label": "Title",
            "description": "Set a title on this element",
            "key": "title",
            "view": "textstring"
        },
        {
            "label": "Custom data",
            "description": "Set the custom data on this element",
            "key": "data-custom",
            "view": "textstring"
        }
    ]


###Full-width settings and styles
It is possible to use settings and styles to add full-width background-images, background-colors and so forth. Just make sure the surrounding *section* is full-width(12 columns by default) and the *rows* inside it will automatically become full-width.