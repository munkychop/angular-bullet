# Angular Bullet

Angular Bullet is an ultra lightweight and simple to use pub-sub library, with AMD/module support and an intuitive API.
It was built to facilitate a simple and consistent system of communication across AngularJS applications and includes only the bare essentials typically needed to achieve this. The main advantage of Angular Bullet over using _$rootScope_ is that it has an **'off'** method, which removes the need to keep track of  and deregister event handlers.

### Usage

You can install via npm with the following command in your command prompt:

```sh
npm install angular-bullet --save
```

Or you can install via Bower instead, if that's your thing:

```sh
bower install angular-bullet
```

Otherwise, simply grab the [source code](https://raw.githubusercontent.com/munkychop/angular-bullet/master/angular-bullet.js) from Github.

### Include Angular Bullet in your application

If using CommonJS then simply require Angular Bullet as per usual, prior to setting up your AngularJS modules:

```javascript
require("angular-bullet");
```

Otherwise use a regular script tag:

```html
<script type="application/javascript" src="path/to/angular-bullet.js"></script>
```

Then ensure that you include Angular Bullet in your module definition:

```javascript
var CoolApp = angular.module("CoolApp", ["angular-bullet"]);
```

You can then inject it where necessary, for example:

```javascript
CoolApp.controller("MainCtrl", ["angularBullet", MainCtrl]);

function MainCtrl (angularBullet)
{
    angularBullet.emit("hello");
}
```


### API

#### **.on()**

```javascript
angularBullet.on("someMessageName", callback);
```

Register a callback function to get called whenever the specified message is triggered.

**Example usage:**

```javascript
function helloCallback () {
    console.log("hello there :)");
}
 
// Register the 'helloCallback' function to be called whenever the 'hello' message is triggered:
    
angularBullet.on("hello", helloCallback);
    

// Somewhere later in the application...
    
    
// Trigger the 'hello' message – Angular Bullet will call the 'helloCallback' function:
    
angularBullet.trigger("hello");
```    

----------

#### **.off()**

```javascript
angularBullet.off("someMessageName"[, callback]);
```

Remove either all callback functions or a specific callback function registered against the specified message.

**Example usage:**

```javascript
function helloCallback () {
    console.log("hello there :)");
}

function anotherCallback () {
    console.log("hello again :)");
}


angularBullet.on("hello", helloCallback);
angularBullet.on("hello", anotherCallback);


// Somewhere later in the application...


// Trigger the 'hello' message – Angular Bullet will call both the 'helloCallback' and 'anotherCallback' functions:

angularBullet.trigger("hello");


// Remove all callback functions associated with the 'hello' message:
angularBullet.off("hello");

// Attempt to trigger the 'hello' message again – Angular Bullet won't call any functions:
angularBullet.trigger("hello");
```

**Example usage removing a specific callback:**

```javascript
function helloCallback () {
    console.log("hello there :)");
}

function anotherCallback () {
    console.log("hello again :)");
}


angularBullet.on("hello", helloCallback);
angularBullet.on("hello", anotherCallback);


// Somewhere later in the application...


// Trigger the 'hello' message – Angular Bullet will call both the 'helloCallback' and 'anotherCallback' functions:

angularBullet.trigger("hello");


// Remove only the 'anotherCallback' function associated with the 'hello' message:
angularBullet.off("hello", anotherCallback);

// Trigger the 'hello' message again – Angular Bullet will only call the 'helloCallback' function:
angularBullet.trigger("hello");
```

----------


    
#### **.once()**

```javascript
angularBullet.once("someMessageName", callback);
```

This function behaves in the same way as the the **'on'** function, except that – once registered – the callback function will only be called a single time when the specified message is triggered.

**Example usage:**

```javascript
function helloCallback () {
    console.log("hello there :)");
}


// Register the 'helloCallback' function to be called whenever the 'hello' message is triggered:

angularBullet.once("hello", helloCallback);


// Somewhere later in the application...


// Trigger the 'hello' message – Angular Bullet will call the 'helloCallback' function:

angularBullet.trigger("hello");


// Attempt to trigger the 'hello' message again – Angular Bullet won't call any functions this time:

angularBullet.trigger("hello");
```  

----------


#### **.trigger()**

```javascript
angularBullet.trigger("someMessageName"[, data]);
```

This function will call all callback functions registered against the specified message, optionally passing in custom data as a payload.

**Example usage:**

```javascript 
function helloCallback () {
    console.log("hello there :)");
}


// Register the 'helloCallback' function to be called whenever the 'hello' message is triggered:

angularBullet.on("hello", helloCallback);


// Somewhere later in the application...


// Trigger the 'hello' message – Angular Bullet will call the 'helloCallback' function:

angularBullet.trigger("hello");
```  

**Example usage with custom data:**

```javascript
function userAddedCallback (data) {
    console.log(data);
}


// Register the 'userAddedCallback' function to be called whenever the 'user-added' message is triggered:

angularBullet.on("user-added", userAddedCallback);


// Somewhere later in the application...


// Create some custom data:

var customData = {
    someProp : "bro",
    someOtherProp : "awesome!"
};


// Trigger the 'user-added' message – Angular Bullet will call the 'helloCallback' function and
// pass in the custom data that you created, which will be sent to the function as a parameter:

angularBullet.trigger("user-added", customData);
```

### Important!
Make sure that you use the **'off'** method to de-register any listeners when AngularJS sends a 'destroy' event to a _$scope_, otherwise Angular Bullet could potentially attempt to call methods that no longer exist.

An easy way to do this is to listen for the 'destroy' event and act on it accordingly:

```javascript
function UserLoginViewController ($scope, angularBullet)
{
    angularBullet.on("someEventName", functionA);
    angularBullet.on("anotherEventName", functionB);

    $scope.$on("destroy", destroyHandler);

    function functionA ()
    {
        // code...
    }

    function functionB ()
    {
        // code...
    }

    function destroyHandler ()
    {
        angularBullet.off("someEventName", functionA);
        angularBullet.off("anotherEventName", functionB);
    }
}
```

## About the Project
Angular Bullet was developed and is currently maintained by [Ivan Hayes](https://twitter.com/munkychop).

Head over to the Github [project page](https://github.com/munkychop/angular-bullet) to find out more. Go ahead and star the project to keep up with its latest developments :-)