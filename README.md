# DOM.create
by Lenin Compres

The DOM.create method creates DOM elements in the *document.body*.
If called before the body is loaded, it waits for the window *load* event before executing.
Click here to learn [what is the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction).

```javascript
DOM.create({
  header: {
    h1: 'Page built with DOM.create'
  },
  main: {
    article: {
      h2: 'Basic DOM created element',
      p: '<b>This</b> is a paragraph.'
    }
  },
  footer: {
    p: 'Made with DOM.create'
  }
});
```
You may provide DOM.create with an element where the model structure should be created.

```javascript
DOM.create({
  h1: 'Hello world',
  p: 'This <b>is</b> a paragraph.'
}, someElement, true);
```

A *true* boolean may be passed to indicate that the new structure should **replace** any existing one, instead of the default **append** mode.
Specify *false* here to **prepend** the structure instead.

You may also provide a *string* to indicate the tag for a new element, where the DOM structure will be created.
The following example creates a *main* element inside the *someElement*. It returns this *main* element.

```javascript
DOM.create({
  h1: 'Hello world',
  p: 'This is <b>a</b> paragraph.'
}, 'main', someElement);
```

DOM.create is agnostic about the order of the arguments that follow the first (model structure):
* An **element** is where the model should be created instead of the *document.body*.
* A **boolean** is a *replace/prepend* flag, instead of the default *append* mode.
* A **string** is the tag for a new element to be created. Or an element **property* to be updated (more on this next).

### Model Properties: Attributes, Events and More

DOM.create recognizes **properties** in the model structure: such an attributes or event handlers.

```javascript
DOM.create({
  input: {
    id: 'myInput',
    placeholder: 'Type value here',
    onchange: (event) => alert(myInput.value)
  },
  button: {
    id: 'goBtn',
    innerText : 'Go',
    addEventListener: {
      type: 'click', 
      listerner: (event) => myInput.value = 'Button pressed'
    }
  }
});

myInput.style.border = 'none';
goBtn.click();
```

NOTE:
* Giving an element an *id:* creates a global variable (with that same name) to hold the element.
* Use *text:* or *innerText:*, *html:* or *innerHTML:*, or simply *content:* for the element's inner content.

### Create, an Element Method

You may invoke the *create* method directly on an element to model it.

```javascript
someElement.create({
  class: 'some-class',
  h1: 'Hello world',
  p: 'This is a <b>paragraph</b>.'
});
```

### The Head Element

Just as any element, you may invoke the **create** method of the head element.

```javascript
document.head.create({
  title: 'Title of the webpage',
  meta: {
    charset: 'UTF-8'
  },
  link : [{
    rel: 'icon',
    href: 'icon.ico'
  }, {
    rel: 'style',
    href: 'style.css'
  }], 
  style: {
    type: 'css',
    content: 'body{ margin:0; backgroundColor: gray; }'
  },
  script: {
    type: 'module',
    src: 'main.js' 
  }
});
```

### Array of Elements

Use arrays to create multiple consecutive elements of the same kind.

```javascript
DOM.create({
  ul: {
    li: [
      'First item',
      'Second item',
      'A third one, for good meassure'
    ]
  }
});
```

Declaring the array inside a *content:* property allows you to set other properties for all the elements in the list.

```javascript
DOM.create({
  ul: {
    li: {
      id: 'listedThings',
      style: 'font-weight:bold',
      height: '20px',
      content : [
        'first item',
        'second item',
        'a third for good meassure'
      ]
    }
  }
});

// Makes the second element yellow
listedThings[1].style.backgroundColor = 'yellow';
```

When *id* is provided, a global variable holding the array of elements is created instead of individual elements. In fact, if you give several ements the same *id*, DOM.create will group them in one global varuable as an array.

## Styling Elements with DOM.create

### Basic Method
Asign a string to the *style:* property to update the inline style of the element—replacing any previous value.

```javascript
document.body.create({
  main:{
    style: 'margin: 20px; font-family: Tahoma; background-color: gray;',
    content: 'The style is in the style attribute of the main element.'
  }
});
```

### Advanced Method
Asign a structural object to the *style:* to update individual style properties—use names (in camelCase).

```javascript
document.body.create({
  main: {
    style: {
      margin: '20px',
      fontFamily: 'Tahoma',
      backgroundColor: 'gray'
    },
    content: {
      h1: 'Styled Main Element',
      p: 'This manages the style values individually.'
     }
  }
});
```

This is equivalent to using the [style property of DOM element](https://www.w3schools.com/jsref/prop_html_style.asp). 

NOTE: Styles may be assigned without an emcompasing *style:* object. The previous code could be written as follows.

```javascript
document.body.create({
  main: {
    margin: '20px',
    fontFamily: 'Tahoma',
    backgroundColor: 'gray',
    h1: 'Styled Main Element',
    p: 'This manages the style values individually.'
  }
});
```

The *style:* and *content:* properties are useful to organize and be specific about the model structure. Yet, **DOM.create** interprets structural properties to match attributes, styles, event handlers and element tags.

### Style Element Method
If the *style:* has a *content:* property, an element with a style tag and CSS content is created. Click here to [learn about CSS](https://www.w3schools.com/css/css_intro.asp).

```javascript
document.body.create({
  main: {
    style: {
      lang: 'scss',
      content: 'main { margin: 20px; font-family: Tahoma; color: gray; }';
    },
    content: 'This style is applied to all MAIN elements in the page.'
  }
});
```

This method is discouraged, since it will affect all elements in the DOM.
Instead, create global styles using **DOM.style**, which adds the CSS to the head, and can interpret structural objects into CSS—nesting and all.

```javascript
DOM.style({
  main: { 
    margin: '20px',
    fontFamily: 'Tahoma',
    color: 'gray'
  },
  'p, article>*': {
    margin: '2em'
  },
  nav: {
    a: {
      backgroundColor: 'silver',
      hover: {
        backgroundColor: 'gold'
      }
    }
  }
};
```

**DOM.style** recognizes pseudo-elements and pseudo-classes when converting CSS.
And selectors containing underscores (\_) are interpreted as periods (.); so, *button_warning:* becomes *button.warning*.

Lastly,

### Element's CSS Method
Use a *css:* property in your model structure to create CSS rules that apply **only** to the current element and its children.

```javascript
document.body.create({
  main: {
    css: {
      margin: '20px',
      fontFamily: 'Tahoma',
      backgroundColor: 'gray',
      a: {
        backgroundColor: 'silver',
        hover: {
          backgroundColor: 'gold'
        }
      }
    },
    nav: {
      a: [
        {
          href: 'home.html',
          content: 'HOME'
        }, {
          href: 'gallery.html',
          content: 'GALLERY'
        }
      ]
    }
  }
});
```

The CSS is added to the document.head's style element under the *id* of the element where it is created.
If the element doesn't have an *id*, a unique one is provided for it.

## Binding Properties

Any element's property (attribute, content, style, content or event handler) can be **bound** to a *Binder* object.
When the *value* property of this object changes, it automatically updates all elements bound to it.

```javascript
let myBinder = DOM.binder('Default value');

DOM.create({
  button: {
    text : 'Go',
    onclick: (event) => myBinder.value = 'Go was clicked.'
  },
  input: {
    value: DOM.bind(myBinder),
  },
  p: {
    content: DOM.bind(myBinder),
  }
});
```

### Binding with a Function

Give binds a functions, so that it returns the correct value to assign to the element's property based on the value of the binder.

```javascript
let fieldEnabled = new Binder(false); // You may create binders using the Binder class as well as DOM.binder().

DOM.create({
  div: {
    style: {
      background: DOM.bind(fieldEnabled, value => value === true ? 'green': 'gray')
    },
    button : {
      text: 'toggle',
      onclick: () => fieldEnabled.value = !fieldEnabled.value
    },
    input: {
      enabled: fieldEnabled.bind(), // You may call the bind method on the binder instead of DOM.bind().
      value: fieldEnabled.bind(value => value ? 'The field is enabled.' : 'The field is disabled.')
    }
  }
});
```

### Binding to Multiple Binders

Provide DOM.bind with an array of binders to create logic based on the values of all binders.

```javascript
enabled: DOM.bind([binder1, anotherBinder], (v1, v2) => v1 && v2 ? 'enabled': 'disabled')
```

## Update Properties with DOM.create

DOM.create allows you to create or modify attributes, styles, event handlers, and content of your elements with just one method and call.

```javascript
myElement.create({
  padding: '0.5em 2em',
  backgroundColor: 'lavender',
  text: 'Some text'
});
```

It even works for single values, indicating the property to be updated in a following *string*.

```javascript
myElement.create('bold', 'fontWeight');

goBtn.create('Go', 'text');

goBtn.create(true, 'disabled');
```

## DOM.create and P5.js

Yes, DOM.create works for P5.js elements. If you are not familiar with P5.js? [Remedy that](https://p5js.org/).

```javascript
p5Element.create({
  h1: 'Hello world',
  p: 'This is a paragraph.'
});
```

When called from p5 or a p5 element, all elements given an id are created as p5 elements, and can execute their p5 methods. 

```javascript
p5.create({
  h1: 'Hello world',
  button: {
    id: goBtn,
    text: 'Go',
    mouseClicked: handlerFunction
  }
);
/* goBtn is a p5 Element. */
```

## Have fun!
