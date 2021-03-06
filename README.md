This plugin is a highly customizable implementation of a lightbox, designed to allow the
display of both static and dynamic content.

You preform the initial setup like this:

```javascript
$("selector").lightbox();
```

By default this binds the triggering of the lightbox to a custom doubletap event, but you
can bind the trigger to any event like this:

```javascript
$("selector").dblclick(function () {
    $(this).lightbox("toggle");
});
```

## options

This plugin lets you specify two options regarding how the content of the lightbox is
displayed.

* scale

> Determines if the conent of the lightbox is fitted to the screen, while preserving
> the aspect ratio of the content.
>
> A value of **true** causes the content of the lightbox to be scaled, the value defaults
> to **false**.

* fullscreen

> Determines if the content of the lightbox is fitted to the screen, overrides the
> value of *scale*.
>
> A value of **true** causes the content of the lightbox to be fullscreen, the value
> defaults to **false**.

* defaultEventHandling

> Determines if the default event handlers are bound, namely that the lightbox will
> toggle on a doubletap.
>
> A value of **true** causes the default event handlers to be bound, the value defaults
> to **true**.

To specify a value for an option you pass an object to the initial setup method with the
name of the option as a key-value pair, like this:

```javascript
$("selector").lightbox({
    fullscreen : false,
    scale : true
});
```

## callbacks

This plugin lets you define callbacks that are fired before and after the lightbox is
opened, closed and resized. In the *close* and *resize* callbacks the value of `this` is
the copy of the **HTMLDOMElement** targeted by your `$("selector")` that is displayed in
the lightbox, but in the *open* callbacks the value of `this` is the original
**HTMLDOMElement** targeted by your `$("selector")` and the copy can be accessed through
`this.data("lightbox").contents`.

The names of the callbacks are as follows:

* preopen
* postopen
* preclose
* postclose
* preresize
* postresize

Defining the callbacks is done in the same manner as the options, like this:

```javascript
$("selector").lightbox({
    fullscreen : true,
    preopen : function () {
        console.log("preopen");
    },
    postopen : function () {
        console.log("postopen");
    },
    preclose : function () {
        console.log("preclose");
    },
    postclose : function () {
        console.log("postclose");
    },
    preresize : function () {
        console.log("preresize");
    },
    postresize : function () {
        console.log("postresize");
    }
});
```

Within the callbacks you can store and retrieve data by attaching it to the target's
lightbox data object like this:

```javascript
$("selector").lightbox({
    preopen : function () {
        console.log(this.data("lightbox").foo); // undefined
        console.log(this.data("lightbox").bar); // undefined
        this.data("lightbox").foo = "foo";
        this.data("lightbox").bar = "bar";
    },
    postopen : function () {
        console.log(this.data("lightbox").foo + this.data("lightbox").bar); // "foobar"
    }
});
```

## reserved words

Each of the above options and callbacks are defined on the target's lightbox data object
and so they should not be modified in any custom callbacks you define. In addition, the
following properties are also used by the plugin, and should not be modified by any
callbacks.

* opened -- **Boolean** that determines if the lightbox is open
* overlay -- **HTMLDOMElement**, div that is the grey background
* box -- **HTMLDOMElement**, div that contains the *contents* and a button to close the lightbox
* contents -- **HTMLDOMElement**, copy of the original target element
* contentWidth -- **Number** that stores the width of *contents*
* contentHeight -- **Number** that stores the height of *contents*
* resizeHandler -- **Function** bound to the window that calls the resize method when needed
