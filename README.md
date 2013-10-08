Backbone.js Patterns
===================

Good practices that our team has learned along the way building Backbone applications.

## Style Guide

### Event Naming Convention

#### empty:Attribute (model)

Documentation pending.

#### notempty:Attribute (model)

Documentation pending.

### Variables and Method Naming Conventions

#### viewData (object)

object containing attributes to be sent to the template

#### removeEl (method)

Used as a proxy for view.remove in cases where you need an animation or perform some action before remove the view.

#### onInitialize (method)

Whenever you extend a Class on Backbone and you want to keep the original behavior on the constructor, use onInitialize.

#### onRender (method)

Whenever you extend a View on Backbone and you want to keep the original behavior on the render method, use onRender.

#### onInvalid (method)

## Patterns

### Model Patterns

#### Model Validation

```js
var Person = Backbone.Model.extend({
	validate: function(attrs, options) {
		this.errors = [];

		if(attrs.name === ''){
		  this.errors.push('error:name:empty');
		}

		if(this.errors.length > 0) return this.errors;
	}
});
```

## Extending Backbone

### Dispatcher

Documentation pending.

### Views

#### dataBinding

One way bind model attributes to a view element.

#### renderElement

Render a piece of the View based on a provided selector.

Rename renderElement to renderEl to keep consistency with backbone styleguide.

```js
this.renderElement('.name');
```

#### registerPartials

Documentation pending.

#### scrollToElement

Documentation pending.
