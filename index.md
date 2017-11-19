# TableDnD
[![npm version](https://badge.fury.io/js/TableDnD.svg)](https://badge.fury.io/js/TableDnD)
[![CDNJS version](https://img.shields.io/cdnjs/v/TableDnD.svg)](https://cdnjs.com/libraries/TableDnD)
[![jsDelivr Hits](https://data.jsdelivr.com/v1/package/gh/isocra/TableDnD/badge?style=rounded)](https://www.jsdelivr.com/package/gh/isocra/TableDnD)

## Installation

The easiest way to install this is using:
```
npm install tablednd
```
or
```
bower install https://github.com/isocra/TableDnD.git
```
Alternatively you can simply reference from CDNJS:
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/TableDnD/0.9.1/jquery.tablednd.js" integrity="sha256-d3rtug+Hg1GZPB7Y/yTcRixO/wlI78+2m08tosoRn7A=" crossorigin="anonymous"></script>
```
or
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/TableDnD/0.9.1/jquery.tablednd.js" integrity="sha256-d3rtug+Hg1GZPB7Y/yTcRixO/wlI78+2m08tosoRn7A=" crossorigin="anonymous"></script>
```
You'll also need to include [jQuery](https://jquery.com) before you include this plugin (so that jQuery is defined).

## Getting Started

Let's create a simple table. The HTML for the table is very straight forward (no Javascript, pure HTML, we haven't added `thead` or `tbody` elements, but it works find with these too):

```html
<table id="table-1" cellspacing="0" cellpadding="2">
    <tr id="1"><td>1</td><td>One</td><td>some text</td></tr>
    <tr id="2"><td>2</td><td>Two</td><td>some text</td></tr>
    <tr id="3"><td>3</td><td>Three</td><td>some text</td></tr>
    <tr id="4"><td>4</td><td>Four</td><td>some text</td></tr>
    <tr id="5"><td>5</td><td>Five</td><td>some text</td></tr>
    <tr id="6"><td>6</td><td>Six</td><td>some text</td></tr>
</table>
```
To add in the "draggability" all we need to do is add a line to the `$(document).ready(...)` function as follows:
```html
<script type="text/javascript">
$(document).ready(function() {
    // Initialise the table
    $("#table-1").tableDnD();
});
</script>
```
Basically we get the table element and call `tableDnD`. If you try this, you'll see that the rows are now draggable.

In the example above we're not setting any parameters at all so we get the default settings. There are a number of parameters you can set in order to control the look and feel of the table and also to add custom behaviour on drag or on drop. 

NB You can also play and experiment with TableDnD using this [jsFiddle](http://jsfiddle.net/DenisHo/dxpLrcd9/embedded/result/). Here you get the documentation, plus live examples that you can modify.

## Configuration Options

You can specify the following properties when initialising the table as follows:
```javascript
    $("#table-1").tableDnD({
      <option>: <value>
    });
```

### autoCleanRelations

Automatically clean up relations between cells and rows etc., (default is `true`)

### autoWidthAdjust

Automatically adjust the width of the first cell (default is `true`)

### dragHandle

This is a jQuery mach string for one or more cells in each row that is draggable. If you
specify this, then you are responsible for setting cursor: move in the CSS and only these cells
will have the drag behaviour. If you do not specify a dragHandle, then you get the old behaviour where
the whole row is draggable.

### hierarchyLevel

Hierarchy level to support parent child. Default is `0` which switches this functionality off.

### indentArtifact

The html artifact to prepend the first cell with as indentation, default `<div class="indent">&nbsp;</div>`

### jsonPretifySeparator

Specify a number (4) as number of spaces or any indent string for use by `JSON.stringify`

### onAllowDrop
Pass a function that will be called as a row is over another row. If the function returns true, allow
dropping on that row, otherwise not. The function takes 2 parameters: the dragged row and the row under
the cursor. It returns a boolean: true allows the drop, false doesn't allow it.

### onDragClass

This class is added for the duration of the drag and then removed when the row is dropped. It is more
flexible than using onDragStyle since it can be inherited by the row cells and other content. The default
is class is tDnD_whileDrag. So to use the default, simply customise this CSS class in your
stylesheet.

### onDragStart

Pass a function that will be called when the user starts dragging. The function takes 2 parameters: the
table and the row which the user has started to drag.

### onDragStop

Pass a function that will be called when the user stops dragging regardless of if the rows have been
rearranged. The function takes 2 parameters: the table and the row which the user was dragging.

### onDragStyle

his is the style that is assigned to the row during drag. There are limitations to the styles that can be
associated with a row (such as you can't assign a border--well you can, but it won't be
displayed, so instead consider using `onDragClass`.) The CSS style to apply is specified as
a map (as used in the jQuery `css(...)` function).
 
### onDrop

Pass a function that will be called when the row is dropped. The function takes 2 parameters: the table
and the row that was dropped. You can work out the new order of the rows by using
table.rows.

### onDropStyle

This is the style that is assigned to the row when it is dropped. As for onDragStyle, there are limitations
to what you can do. Also this replaces the original style, so again consider using onDragClass which
is simply added and then removed on drop.
 
### scrollAmount

This is the number of pixels to scroll if the user moves the mouse cursor to the top or bottom of the
window. The page should automatically scroll up or down as appropriate.

### sensitivity

The minimum mouse movement before a drag is triggered, default `10`

### serializeParamName

If you want to specify another parameter name instead of the table ID, default is `false` i.e. use the table ID.

### serializeRegexp

Allows you to filter which rows are serialised and which aren't. If the row ID matches this regex, then include it,
otherwise not. Default is `/[^\-]*$/`

## Other ways to control behaviour

Add `class="nodrop"` to any rows for which you don't want to allow dropping, and `class="nodrag"` to any rows
that you don't want to be draggable.

Inside the onDrop method you can also call $.tableDnD.serialize() this returns a string of the form
`<tableID>[]=<rowID1>&<tableID>[]=<rowID2>` so that you can send this back to the server. The table must have
an ID as must all the rows.

 
## Other methods:

`$("...").tableDnDUpdate()`
Will update all the matching tables, that is it will reapply the mousedown method to the rows (or handle cells).
This is useful if you have updated the table rows using Ajax and you want to make the table draggable again.
The table maintains the original configuration (so you don't have to specify it again).

`$("...").tableDnDSerialize()`
Will serialize and return the serialized string as above, but for each of the matching tables--so it can be
called from anywhere and isn't dependent on the currentTable being set up correctly before calling
