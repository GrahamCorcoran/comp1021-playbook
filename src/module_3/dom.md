# The HTML Document Object Model (DOM)

HTML is the markup language that represents the objects on our page, but that only gives us half of what we need to make an interactive website. Interactivity requires modifications, and in order to be able to modify our HTML, we need a standard way to refer to these HTML elements and interact with them. This is where the DOM comes into play. The DOM is created by the browser when a page is loaded, and represents all of the content, structure, and style of the page. You can think of the DOM as the *interface* between the HTML, and the outside world. JavaScript is the programming language we use to manipulate the DOM.

# DOM Events
When the user interacts with the DOM, this triggers *events* to fire. All an event is referring to is a thing that happened. For example, the most obvious event is a ``click`` event, which is triggered when a user uses their mouse to click something. 

## Common types of DOM Events
There are [many, many events](https://developer.mozilla.org/en-US/docs/Web/Events) referring to all sorts of different things that can happen during a user's journey through a webpage. Modern browsers are very powerful and so you can interact with events in many ways, with the intent of making it easier for people to use the internet.

### 1. Mouse Events 
Triggered by mouse actions such as clicks, movement, or hovering.
- `click`: Fires when an element is clicked.
- `dblclick`: Fires when an element is double-clicked.
- `mouseover`: Fires when the mouse enters an element.

### 2. Keyboard Events
Triggered when the user interacts with the keyboard.
- `keydown`: Fires when a key is pressed down.
- `keyup`: Fires when a key is released.

### 3. Form Events
Triggered by interactions with form elements.
- `submit`: Fires when a form is submitted.
- `change`: Fires when an input value changes (after losing focus).
- `input`: Fires when the value of an input changes (in real-time).
- `focus`: Fires when an element gains focus.
- `blur`: Fires when an element loses focus.

### 4. Window Events
Triggered by actions on the browser window.
- `load`: Fires when the whole page (including all resources) is loaded.
- `resize`: Fires when the window is resized.
- `scroll`: Fires when the page is scrolled.

### 5. Clipboard Events
Triggered when users copy, cut, or paste content.
- `copy`: Fires when content is copied.
- `cut`: Fires when content is cut.
- `paste`: Fires when content is pasted.