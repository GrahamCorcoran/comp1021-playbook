# Event Handling
Since the DOM provides us with events for almost any type of user interaction imaginable, we have lots of options for what we might want to *do* with those events. This is the basis for **Event Handling**. When we have an event that triggers, and we do something in response, we have handled that event.

## Event Handling Flow
In order to handle events with JavaScript, we need to do a few things to handle an event.

```admonish tip
The steps below might seem complicated at first, but think about it like any other type of event in real life, and it's not so bad. You need to know **what** you're responding to, **where** it happens, and what your **response** will be. It's like hearing a doorbell and deciding to answer the door:

1. What am I expecting to fire an event -> The doorbell.
2. What event am I listening for? -> The ring.
3. What am I going to do about it? -> Answer the door.
```

| Step | Context      | Action                                                                 |
|------|--------------|------------------------------------------------------------------------|
| 1    | `HTML`       | Understand and create (if needed) the *element* to respond to.         |
| 2    | `HTML`       | Give the element an `id` or `class` so it can be referenced in JavaScript.     |
| 3    | `JavaScript` | Create the *action* to take after the event occurs.                    |
| 4    | `JavaScript` | Use `document.querySelector()` or similar to select the element.       |
| 5    | `JavaScript` | Add an event listener linking the element, event type, and the action. |
| 6    | `The Browser`| The event fires.                                                       |
| 7    | `JavaScript` | The event listener calls the function—handling is complete!            |

## Selectors
In order to attach an event listener to an element, we need to first select the element so that JavaScript knows what we're talking about. Just like in CSS, we can use the same type of selectors to work with elements in JavaScript. It's important that you're comfortable using the [querySelector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) method, and [getElementById](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById).

```javascript
let button = document.querySelector('#clickyBtn');
```

## Callback Functions
A callback function is most simply described as a function that is called once another function is complete. In the case of our event listeners, we pass a callback function to it to say "When you see the event has happened, call this function." When passing callback functions, you do **not** need to call them. You pass the function name without the parameters.

```javascript
myFunction   // Yes
myFunction() // No
```

## Registering the Event Listener
When we have an element to work with, we can register, or add the event listener using the ``addEventListener`` method. This method is a function of the *element*, and so it is accessed using the syntax:

```javascript
button.addEventListener('event', callback);
```

### Anonymous Callback Functions
Both here and in the [example](event_handling_example.md) you will see us pass named functions as a callback. However, JavaScript allows you to create anonymous functions on-the-fly, which you will see fairly regularly especially in the context of event handling. For example, we could choose not to name a function that does nothing except log a few messages to console, and instead write the following

```javascript
button.addEventListener('click', () => {
    console.log("I have been clicked!");
    console.log("Please stop clicking me.");
})
```