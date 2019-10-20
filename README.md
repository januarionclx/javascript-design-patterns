# Javascript Design Patterns

## Introduction

>Design patterns are typical solutions to common problems in software design. 
>Each pattern is like a blueprint that you can customize to solve a particular design problem in your code.

>Patterns are a toolkit of solutions to common problems in software design. 
>They define a common language that helps your team communicate more efficiently.
@refactoring.guru

I'll be adding more and more patterns over time which you can use as a reference for your own JavaScript apps.

### Module Pattern (using IIFE's)

```javascript
const ATM = (function (){
    let credit = 0;

    function depositPrivate(n) {
        credit += n;
    }

    return {
        deposit: function (n) {
            depositPrivate(n);
            console.log(
                `Hey! Thanks for depositing ${n} in our bank.\n` +
                `The deposit has been successfully processed.`);
        },
        
        show: function () {
            console.log(`Your credit: ${credit}`);
        }
    }
})()
```

### Module Pattern (using Arrow functions)

```javascript
const ATM = (() => {
    let credit = 0;

    const depositPrivate = n => {
        credit += n;
    }

    return {
        deposit: n => {
            depositPrivate(n);
            console.log(
                `Hey! Thanks for depositing ${n} in our bank.\n` +
                `The deposit has been successfully processed.`);
        },
        
        show: () => {
            console.log(`Your credit: ${credit}`);
        }
    }
})()
```

### Module Pattern using file encapsulation (using NodeJS module.exports)

// ATM.js

```javascript
let credit = 0;

const depositPrivate = n => {
    credit += n;
}

//nodeJS
module.exports = {
    deposit: n => {
        depositPrivate(n);
        console.log(
            `Hey! Thanks for depositing ${n} in our bank.\n` +
            `The deposit has been successfully processed.`);
    },
    
    show: () => {
        console.log(`Your credit: ${credit}`);
    }
}
````    

// index.js

```javascript
const ATM = require('./ATM.js');

console.log(ATM) // object
ATM.show()     //? 0
ATM.deposit(5)
ATM.show()     //? 5
```

### The Observer Pattern

Instead of prividing an incomplete example of the Observer Pattern, I decided that it would be more benefitial to provide a fully working example of an event handler class (similar to [NodeJS's events.js](https://github.com/Gozala/events), just simpler for the sake of your own understanding and the objective of this guide).

In this example, the class EventHandler would be what they often refer to the **Subject Class** (which we'll use later to define our **Subject**).

It cointains the list of the **Observers** (events in this example) and methods to facilitate adding/removing Observers (again, events) to the list.

The **Observers** (events) have a function signature that will be invoked when the **Subject** changes (i.e. when an event occurs / has been emited.)

Here's the full class providing examples for:

* on(event, callback)
* once(event, callback)
* off(event, callback)
* emit(event, ...args)

```javascript
class EventHandler {
    constructor(){
        this._listeners = {};
    }
        
    on(event, callback){
        this._listeners[event] = this._listeners[event] || [];
        this._listeners[event].push(callback);

        return this;
    }

    once(event, callback){
        this._listeners[event] = this._listeners[event] || [];

        let onceCallback = () => {
            callback();
            this.off(event, onceCallback);
        }

        this._listeners[event].push(onceCallback);
        return this;
    }

    off(event, callback){
        let listener = this._listeners[event];
        if (!listener)
            return this;

        for (let i = listener.length-1; i >= 0; i--){
            if (listener[i] === callback){
                listener.splice(i, 1);
                break;
            }
        }

        return this;
    }

    emit(event, ...args){
        let listener = this._listeners[event];
        if (!listener)
            return false;

        listener.forEach((callback) => {
            callback(...args);
        });

        return true;
    }
}
```

And here's the implementation example:

```javascript
// Declare our Subject
const subject = new EventHandler();

// Declare signature functions for our Observers (events)
const callback = function(data){ 
    console.log('Callback!');
}
const callback2 = function(){ 
    console.log('Callback 2!');
}

// Add Observers (events) to our Observer List.
subject.on('message', callback)
subject.on('message', callback2)


// Emit the events
subject.emit('message');

// Remove an Observer from the Observer List.
subject.off('message', callback);

// Emit the event again to check if the previous Observer has been succesfuly removed.
subject.emit('message')
```