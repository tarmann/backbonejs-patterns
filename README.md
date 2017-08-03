Backbone.js Patterns
===================

Good practices that our team has learned along the way building Backbone applications.

**Related Articles**

* [Smashing Magazine: Backbone.js Tips And Patterns](http://coding.smashingmagazine.com/2013/08/09/backbone-js-tips-patterns/)
* [Rico Sta. Cruz: Backbone patterns](http://ricostacruz.com/backbone-patterns/)

## Style Guide

### Event Naming Convention

Here's the list of custom events that can be used with any Model.

* empty:\[attribute\] (model) — when a specific attribute becomes empty.
* notempty:\[attribute\] (model) — when a specific attribute is not empty anymore.

### Variables and Method Naming Conventions

#### viewData (object)

Object containing attributes to be sent to the template.

```js
var Bookmark = Backbone.View.extend({
  template: _.template(…),
    render: function () {
      var viewData = _.extend(this.viewOptions, this.model.toJSON());
      this.$el.html( this.template( viewData ) );
      return this;
    }
});
```

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

#### Persisting an Collection

Whenever you need to save an entire Collection, you should have a Model as wrapper that will be the context. Than you can save it as an attribute. You can have a Backbone Collection as a property of the Model and bind it to the attribute to reflect changes.

Use set instead of reset when the collection changes on the attributes, this will trigger all the events like change, remove and add on the models.

The `remove` event on the Collection doesn't trigger `destroy` on the Model. So if you need to have it reflected on a model that is binded to a view, you should trigger `remove` on the model and listen on the view.

```js
var MyModel = Backbone.View.extend{    
	initialize: function(){
	    this.elementsCollection.on({
	        'remove': this.onRemoveElement
	    }, this);
	},	

    onRemoveElement: function(model, collection, options){
        model.trigger('remove', model);
    },

    updateElementsCollection: function(model){
        this.elementsCollection = this.elementsCollection || new SD.Collection.ErfxElement();
        this.elementsCollection.set( this.get('Elements') );
        this.elementsCollection.parentModel = this;
    }
});
```

#### Persisting an Collection (Alternative)

As alternative, save the collection by extending Backbone.

```js
var Groups = Backbone.Collection.extend({
  model: Group,
  url: function() {
    return '/groups/'; 
  },
  save: function(){
    Backbone.sync('create', this, {
      success: function() {
        console.log('Saved!');
      }
    });
  }
});
```

```
Send updated collection of elements as an attribure
PUT collection/{id} - Model (Subcollection)
{ Subcollection: [ { ... }, { ... } ] }

Send only new elements as collection to elements resource
POST collection/{id}/subcollection (Collection)
[ { ... }, { ... } ]

Update entire collection sending it to elements resource
PUT collection/{id}/subcollection (Collection)
[ { id: 1 }, { id: 2 }, { ... }, { ... } ]
```

***Related links***

* ["How" to save an entire collection in Backbone.js - Backbone.sync or jQuery.ajax?](http://stackoverflow.com/questions/6879138/how-to-save-an-entire-collection-in-backbone-js-backbone-sync-or-jquery-ajax)
* [How to design a RESTful collection resource](http://stackoverflow.com/questions/2810652/how-to-design-a-restful-collection-resource)
* [Backbone.js Save Collections](http://c2journal.com/2013/03/23/backbone-js-save-collections/)

### View Patterns

#### Render Element Attributes

```js
var Person = Backbone.View.extend({	
	renderAttr: function(){
		var attrs = {
			'id': 'my-element-' + this.model.attributes.id,
			'data-id': this.model.attributes.id,
		};

		_.each(attrs, function(value, name){
			this.$el.attr(name, value);
		}, this);
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
