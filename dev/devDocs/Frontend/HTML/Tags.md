### HTML `<input>` Tag

The `<input>` tag is a key element for creating interactive web forms. While it can exist on its own, it's most useful when combined with several core attributes.

#### Core Attributes

- **`type`**: This is the most essential attribute. It tells the browser what kind of input control to display, such as a simple text box (`type="text"`), a button (`type="submit"`), a checkbox (`type="checkbox"`), or a password field (`type="password"`).
- **`name`**: This attribute is crucial for form submission. It acts as a unique identifier for the input's data. When the form is submitted, the data is sent as a key-value pair, where the `name` is the key.
- **`value`**: This sets the initial, default value of the input field. For text inputs, it's the text that appears in the box. For radio buttons and checkboxes, it's the value that is sent to the server if the item is selected.
#### Other Useful Attributes

- **`id`**: Provides a unique identifier for the element, which is useful for styling with CSS or interacting with JavaScript.
- **`placeholder`**: Displays a short hint in the input field before the user types anything.
- **`required`**: A boolean attribute that makes the input field mandatory. The user cannot submit the form without filling it in.
- **`disabled`**: A boolean attribute that makes the input non-interactive and prevents its value from being submitted.