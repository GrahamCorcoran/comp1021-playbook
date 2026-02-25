# Creating an Event Handler

Let's build an example event handler. To keep things simple, we'll use a button as the element we plan to respond to. The event we will listen for is a click event, meaning someone has clicked our button. When the button is clicked, we will respond by changing the colour of an h1 element above the button to something else. We'll refer to the chart from the [event handling](event_handling.md) page to ensure we did all the steps correctly as we go.

| Step | Context      | Action                                                                 |
|------|--------------|------------------------------------------------------------------------|
| 1    | `HTML`       | Understand and create (if needed) the *element* to respond to.         |
| 2    | `HTML`       | Give the element an `id` or `class` so it can be referenced in JavaScript.     |
| 3    | `JavaScript` | Create the *action* to take after the event occurs.                    |
| 4    | `JavaScript` | Use `document.querySelector()` or similar to select the element.       |
| 5    | `JavaScript` | Add an event listener linking the element, event type, and the action. |
| 6    | `The Browser`| The event fires.                                                       |
| 7    | `JavaScript` | The event listener calls the function—handling is complete!            |

## Step-by-step Example
Starting with the HTML file, we need to:
### 1. Understand and create the *element* to respond to.

```html
<!-- For page readability sake, I've cut out most of this file except the body. -->
<body>
    <!-- We know we will want to modify the h1, so let's add it. -->
    <h1>This is a title with boring, black text.</h1> 

    <!-- The button we will respond to. -->
    <button>Click me!</button>
</body>
```

### 2. Give the element an `id` or `class` so it can be referenced in JavaScript.

Once we have this done we are ready to get started, but we'll notice very quickly it's hard to reliably select an element in JavaScript without some way to tell it apart from other elements. If I create another button in the future, I probably don't want that second button to also change the colour of the h1.

To the point of the doorbell analogy from before, we need to specify which doorbell we're responding to -- it would be quite tiring if *every single doorbell in our city* rang in our house.

```html
<body>
    <h1>This is a title with boring, black text.</h1> 

    <!-- Here we've added an id to be able to easily select. -->
    <button id='colorChangeBtn'>Click me!</button>
</body>
```

~~~admonish warning title="Don't forget to include your JavaScript file!"
Don't forget! If you don't include your JavaScript file in your HTML using the ``<script>`` tag, it won't execute! This is a common mistake and you can spend a lot of time trying to fix it before you realize.

```html
<body>
    
    ... other code ...

    <!-- We include this at the bottom to ensure it's
         loaded after all our elements. We'll handle this
         a bit more gracefully in the future. -->
    <source src="event-demo.js"></source>
</body>
```
~~~

### 3. Create the *action* to take after the event occurs. 
The next step we'll take is creating the thing that we will do in response to the action. I like to recommend thinking about your response first, because it can be easier to ensure it works separately. It's best to know that your code works *before* you add it to an event listener, so you are only debugging one thing at a time. 

```javascript
function changeTitleColor() {
    // Let's assume for now that we only have a single h1 element on the page.
    let title = document.querySelector('h1');

    // With this selected, now we can modify the title properties to change its color.
    title.style.color = '#FF0000';
}

// Let's make sure our function works by calling it. Once we know it works, we'll remove this line.
changeTitleColor();
```

### 4. Use document.querySelector() or similar to select the element.
We saw this already in use in the previous step, but it's important to call out specifically because it gets forgotten a lot! We need a way to actually access the button to apply the event listener, so let's ensure we have it named as a variable.

```javascript
let button = document.querySelector('#colorChangeBtn');

// By logging the button, you can ensure that you correctly selected it. We can remove this after.
console.log(button) 

function changeTitleColor() {
    // function code here
}
```

### 5. Add an event listener linking the element, event type, and the action.

At this point, we have our button accessible to JavaScript via a variable. We also have the action we want to take when the button is clicked. So now all we have to do is register the event listener.

```javascript
// .. the rest of our code ..

button.addEventListener('click', changeTitleColor);
```

```admonish note
There's something important to notice in the above code. We didn't *call* the function when we passed it to the addEventListener function. Instead, we just passed the function as a parameter. This is because we don't want to change the color right away, instead we want the event listener to call this function in the future if the button is clicked.
```

### And, done!

Now if we open the HTML file in the browser, we'll notice that our h1 is still a boring black color. However, when we click the button, our h1 changes to bright red! 

Our completed code is below. Feel free to copy it into your own editor and experiment with it. Even if you feel you understand, experimentation is the best way to solidify your learning.

``event-demo.html``
```html
<!DOCTYPE html>
<body>
    <h1>This is a title with boring, black text.</h1>
    <button id="colorChangeBtn">Click me!</button>

    <script src="event-demo.js"></script>
</body>
</html>
```

``event-demo.js``
```javascript
let button = document.querySelector('#colorChangeBtn');

function changeTitleColor() {
    let title = document.querySelector('h1')

    title.style.color = '#FF0000';
}

button.addEventListener('click', changeTitleColor);
```