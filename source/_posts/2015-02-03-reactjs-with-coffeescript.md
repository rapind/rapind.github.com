---
layout: post
title: "Using CoffeeScript with ReactJS"
date: 2015-02-03 9:23
comments: true
categories: [programming, javascript, coffeescript, reactjs]
published: true
---

Lately I've been using ReactJS a lot to build rich user experiences on the web, and it's been absolutely great. A huge improvement over AngularJS in my humble opinion.</p>

The only ugly spot with ReactJS is JSX. I can see the appeal of using declarative HTML for you templates for readability, but having switched to HAML (and then Slim and Jade) something like 6 years ago, writing HTML feels like a huge step backwards.

Luckily, using coffescript for my ReactJS components and eschewing JSX entirely, we can accomplish a syntax that's very similar to HAML / Slim / Jade. If you're not a fan of CofeeScript, HAML variants, or significant whitespace, then I recognize that there's very little chance I'll be able to convince you otherwise. However if you are a fan of any of those, then check this out.

Here's the plain Javascript version of a simple component. It's pretty verbose.

```javascript
var Jumbotron = React.createClass({
  render: function() {
    return (
      React.createElement('div', {className: "jumbotron"},
        React.createElement('div', {className: "container"},
          React.createElement('h1', {},
            React.createElement('p', {}, "This is a template for a simple marketing or informational website.")
            React.createElement('p', {}, 
              React.createElement('a', { className: "btn btn-primary btn-lg", href: "#", role: "button" }, "Learn more »")
            )
          )
        )
      )
    );
  }
});
```

Here's the JSX version of the same component. Quite an improvement I think, but it introduces a new syntax that seems a bit messy to me and most likely throws off your editor's syntax highlighting and functions.

```javascript
var Jumbotron = React.createClass({
  render: function() {
    return (
      <div className="jumbotron">
        <div className="container">
          <h1>Hello, world!</h1>
          <p>This is a template for a simple marketing or informational website.</p>
          <p><a className="btn btn-primary btn-lg" href="#" role="button">Learn more »</a></p>
        </div>
      </div>
    );
  }
});
```

Finally, here's the CoffeeScript version of the component. At least as succinct as the JSX version.

```coffeescript
{ div, h1, p, a } = React.DOM

Jumbotron = React.createClass
  render: ->
    div className: "jumbotron",
      div className: "container",
        h1 {}, "Hello World"
          p {}, "This is a template for a simple marketing or informational website."
          p {},
            a
              className: "btn btn-primary btn-lg"
              href: "#"
              role: "button"
              "Learn more »"
```

For the sake of completeness, here's a CJSX version (CoffeeScript + JSX). Even more succinct, however again we're introducing a new syntax within our CoffeeScript which I'm not a fan of.
```coffeescript
Jumbotron = React.createClass
  render: ->
    <div className="jumbotron">
      <div className="container">
        <h1>Hello, world!</h1>
        <p>This is a template for a simple marketing or informational website.</p>
        <p><a className="btn btn-primary btn-lg" href="#" role="button">Learn more »</a></p>
      </div>
    </div>
```

Now unfortunately it's not all puppy dogs and ice cream. There are a few gotchas with the CoffeeScript version.

#### Gotcha 1: Optional Curly Braces
CoffeeScript allows you to omit Curly braces on hashes. This can cause readability issues for the next person who comes along to read your code.

```coffeescript
{ div, h1, p, a } = React.DOM

Jumbotron = React.createClass
  render: ->
    div { className: "jumbotron" },
      div { className: "container" },
        h1 {}, "Hello World"
          p {}, "This is a template for a simple marketing or informational website."
          p {},
            a {
              className: "btn btn-primary btn-lg"
              href: "#"
              role: "button"
              "Learn more »" }
```

#### Gotcha 2: Commas and New lines
CoffeeScript allows you to omit commas between hash assignments and opt instead for indented new lines. Again this can cause readability issues, especially when combined with Gotcha #1 above.

```coffeescript
{ div, h1, p, a } = React.DOM

Jumbotron = React.createClass
  render: ->
    div
      className: "jumbotron",
      div
        className: "container",
        h1
          {}
          "Hello World"
          p
            {}
            "This is a template for a simple marketing or informational website."
          p
            {}
            a
              className: "btn btn-primary btn-lg"
              href: "#"
              role: "button"
              "Learn more »"
```

Ultimately if you are going to use CoffeeScript for your React components instead of JSX (and I recommend you do), then it's probably a good idea to agree upon some conventions with your team on when braces and commas are used. My preference has been to use braces for single line hash assignments, and I'm considering enforcing braces for multiple line attribute assignments w/ React to better separate them from the next element.
