# ejs-mate

## About

Express 4.x `layout`, `partial` and `block` template functions for the EJS template engine.

Previously also offered `include` but you should use EJS 1.0.x's own method for that now.

## Installation

```bash
$ npm install ejs-mate --save
```

(`--save` automatically writes to your `package.json` file, tell your friends)

## Usage

Run `node app.js` from `examples` and open `localhost:3000` to see a working example.

Given a template, `index.ejs`:

```html
<% layout('boilerplate') -%>
<h1>I am the <%=what%> template</h1>
```

And a layout, `boilerplate.ejs`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>It's <%=who%></title>
  </head>
  <body>
    <section>
      <%-body -%>
    </section>
  </body>
</html>
```

When rendered by an Express 4.0 app:

```js
var express = require('express'),
  engine = require('../'),
  app = express();

// use ejs-locals for all ejs templates:
app.engine('ejs', engine);

app.set('views',__dirname + '/views');
app.set('view engine', 'ejs'); // so you can render('index')

// render 'index' into 'boilerplate':
app.get('/',function(req,res,next){
  res.render('index', { what: 'best', who: 'me' });
});

app.listen(3000);
```

You get the following result:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>It's me</title>
  </head>
  <body>
    <section>
      <h1>I am the best template</h1>
    </section>
  </body>
</html>
```

Note, if you haven't seen it before, this example uses trailing dashes in the EJS includes to slurp trailing whitespace and generate cleaner HTML. It's not strictly necessary.

## Features

### `layout(view)`

When called anywhere inside a template, requests that the output of the current template be passed to the given view as the `body` local. Use this to specify layouts from within your template, which is recommended with Express 3.0, since the app-level layout functionality has been removed.

### `partial(name,optionsOrCollection)`

When called anywhere inside a template, adds the given view to that template using the current given `optionsOrCollection`. The usual way to use this is to pass an Array as the collection argument. The given view is then executed for each item in the Array; the item is passed into the view as a local with a name generated from the view's filename.

For example, if you do `<%-partial('thing',things)%>` then each item in the `things` Array is passed to `thing.ejs` with the name `thing`. If you rename the template, the local name of each item will correspond to the template name.

## Running Tests

To run the test suite first invoke the following command within the repo, installing the development dependencies:

```bash
$ npm install -d
```

then run the tests:

```bash
$ npm test
```

## Whither Include?

Previous versions of this library had an `include` function. This is now supported directly by EJS, albeit with a different syntax. For `ejs-locals` 1.0+ simply do:

### `<% include path/view %>`

When called anywhere inside a template, this adds the given view to that template using the current options and locals. This is built-in to [EJS](https://github.com/visionmedia/ejs) 1.0+.

## Credits

This library is a fork from Robert Tom Carden's [ejs-locals](https://github.com/RandomEtc/ejs-locals), and the partial function remains relatively untouched from there (aside from cache support). Robert is still updating his library and it now supports other template engines - check it out!

## License

(The MIT License)

Copyright (c) 2012 Robert Sk&ouml;ld &lt;robert@publicclass.se&gt;
Copyright (c) 2012 Tom Carden &lt;tom@tom-carden.co.uk&gt;
Copyright (c) 2014 Jackson Tian &lt;shyvo1987@gmail.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
