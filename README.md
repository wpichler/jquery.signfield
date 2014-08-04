# jQuery signature field

Provides signature field as jQuery component, using either a sketch pad or an uploaded signature file.

See [demo](http://rawgit.com/applicius/jquery.signfield/master/demo.html).

> Requires jQuery 1.10+ & [sketch.js](http://intridea.github.com/sketch.js).

## Setup

Following scripts must be included at bottom of page body.

```html
<script src="jquery.js"></script>
<script src="sketch.min.js"></script>

<!-- Localized messages -->
<script src="lang/jquery.signfield-en.min.js"></script>

<!-- Component script -->
<script src="jquery.signfield.min.js"></script>
```

## JavaScript usage

Any element can be turned into a signature field.

Consider element thereafter as original one you want to replace with a signature field.

```html
<div id="uniqueId" class="classes" data-name="fieldName" 
  data-max-size="2048" data-pen-tickness="3" data-pen-color="red" ></div>
```

- `id` (optional): Signature field identifier.
- `class` (optional): CSS classes, if present applied on signature field.
- `data-name` (required): Field name, used on submit.
- `data-max-size` (optional): If present, used as limit to upload signature file.
- `data-pen-thickness` (optional, default = 2): If present and if sketch (canvas) is supported, defines pen thickness.
- `data-pen-color` (optional): If present and if sketch is supported, defines pen color.

Then calling `$("#uniqueId").signField()` will replace original element with a signature field as following (considering `${orig.x}` is value of attribute `x` on original element).

```html
<div id="${orig.id}" class="${orig.class}">
  <!-- If canvas is supported by browser -->
  <label class="radio sketch-radio">
    <input type="radio" name="${orig.name}-type" value="canvas" />
    <!-- + Localized label -->
  </label>
  <a class="clear-canvas"><!-- Localized message --></a>
  <canvas></canvas>

  <label class="radio file-radio">
    <input type="radio" name="${orig.name}-type" value="file" />
    <!-- + Localized label --> 
  </label>
  <!-- end of if -->

  <input type="file" name="${orig.name}-file" />
  <span class="message"></span>
</div>
```

> Canvas size is defined by CSS, you may want to enforce it so that size of sketch image data is compliant with what you expect (e.g. `#uniqueId canvas { width: 200px; height: 100px }` ensure sketch image is 200x100).

If signature component wants to raise an error, CSS class `has-error` is added to the nesting `div` element, and localized message written in its `span.message`.

### Methods

#### errors

`.signField('errors')`

If there is error for a signature field, returns array of error keys or `[]` (if none).

```javascript
var errs = $('#signature').signField('error');
// #signature should have been previously set up as signature field
```

### Events

`change`

Fired when field value is changed, either by uploading a signature file or by sketching one.

```javascript
$("#signature").on('change', function() { 
  var signature = $(this);
  // ...
})
```

## Submit

When form including a signature field is submitted, either canvas image data or uploaded file is sent as field value.

A related field, suffixed with `-type` is also submit, with either "canvas" or "file" as value, so that you can process signature data accordingly.

> Canvas image is submitted as PNG data, using [data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/data_URIs) format.

## Localization

Messages are provided by language pack in separate file 
(e.g. For english, `lang/jquery.signfield-en.min.js`).

If you find a language pack is missing, please [file a ticket](https://github.com/playframework/playframework/issues).

To customize some messages, you can redefine them in loaded language pack:

```javascript
signField_I18N['sketch.label'] = "Custom label for Sketch radio button";
```
