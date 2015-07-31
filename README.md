fake-terminal
===========

An extensible pseudo-terminal web component for your website (or whatever you do in the browser), based on [terminal.js](https://github.com/avgp/terminal.js).

## What's this?
Terminal.js is a Javascript terminal emulation for use in the browser, bundled as a web component.

I have a [live demo here](http://avgp.github.io/fake-terminal/index.html)

You can do a bunch of things with it:

- Create a CLI-style API interface that runs in the browser
- Create a remote terminal emulator for something that exposes an interface in a browser-consumable way (CORS, Websocket, WebRTC, ...)
- Create a text-based adventure game in the browser
- whatever you can come up with, where a command line interface is useful.

## How do I use it?
It's really easy:

1. Add a `<link rel="import" href="https://fe1a7206bd2241e9262755c6f3d895bed9fd49d2.googledrive.com/host/0B9MEoZDi5-peaWNXWDhwNHNUTDA/fake-terminal.html">` to your `<head>` (or host that file yourself)
2. Add a `<fake-terminal>$</fake-terminal>` to your page body (you can customize the terminal prompt by using different content instead of $)
3. Create commands as methods of a javascript object and use the `commands` property to expose them, like explained below.

## Extensible command interface

The terminal is only a way to interact with "commands" and "commands" are a bundles of functionality.  
So to use the terminal, you'll need to create a bunch of functions that actually do something - and that's not hard.

### A greeting command
So let's build a command that greets the user with the name she enters, like this:

```bash
$ hello Alice
Hi there, Alice
```

in Terminal.js this is done by creating a ``commands`` object and add a "hello" method to it.  
That method will take one parameter, which will be the array of arguments (separated by spaces) entered to call the command and returns HTML to be displayed in the terminal.

```javascript
var commands = {
  hello: function(args) {
    if(args.length < 2) return "<div>Please tell me your name like this: <pre>hello Alice</pre></div>";
    return "<div>Hi there, " + args[1] + "</div>";
  }
};
```

Note that the ``args`` array's first element is the name of the command itself.

Now we can make our terminal:

```html
  <!doctype html>
  <html>
    <head>
      <link rel="fake-terminal.html">
    </head>
    <body>
      <fake-terminal id="myterm">guest@terminal.js &sim; &gt;</fake-terminal>
      <script>
        var cmds = {}, terminal = document.getElementById("myterm");
        cmds.hello = function(args) { return "<h1>Hello, " + (args[1] || "unknown") + "</h1>"; };
        if(terminal.ready) {
          // the native web components implementation was faster :-)
          terminal.commands = cmds
        } else {
          // the polyfill needs some more time..
          terminal.onload = function() {
            this.commands = cmds;
          }
        }
      </script>
    </body>
  </html>
```
and we're done. We have a terminal that can greet the user :)

### A full example
The code for the [live demo](http://avgp.github.io/fake-terminal) lives in index.html.

# License
MIT License - basically: Do whatever you feel like, but don't sue me when it blows up.
