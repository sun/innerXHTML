# innerXHTML

A JavaScript library to read and write DOM elements in XHTML, mostly utilizing native browser methods.

innerHTML produces invalid XHTML.  Instead of a big string, innerXHTML reads and writes the DOM tree, node by node.

## Usage

### Functions

#### `innerXHTML(Node|String node)`

Reads a DOM tree of Node or the DOM node identified by the HTML ID string, and returns a text string of the contained XHTML markup.

#### `innerXHTML(Node|String node, String htmlString [, mixed prependTo = false])`

Writes a DOM tree from a passed in `htmlString` containing XHTML markup to the `Node` or the DOM node identified by the HTML ID string.

The optional `prependTo` argument dictates where the markup is inserted:

- `false`: (default) Replaces all children of `Node`.
- DOMNode: A DOM node. htmlString is inserted before it.
- ID: A HTML ID string suitable for document.getElementById(). htmlString is inserted before it.
- `true`: Appends htmlString to `Node`.

#### `translateXHTML(String htmlString)`

Translates a string containing XHTML and XML markup into nodes for insertion into the DOM.

### Reading and parsing DOM elements

With innerHTML you'd do:

    var container = document.getElementById('container');
    var code = container.innerHTML;

With innerXHTML:

    var container = document.getElementById('container');
    var code = innerXHTML(container);

This returns the content of `#container` as a XHTML markup string.

### Writing and serializing DOM elements

With innerHTML you'd do:

    var container = document.getElementById('container');
    var markup = '<div id="content"><p class="text">Hello world!</p></div>';
    var code = container.innerHTML(markup);

With innerXHTML:

    var container = document.getElementById('container');
    var markup = '<div id="content"><p class="text">Hello world!</p></div>';
    var code = innerXHTML(container, markup);

This replaces the content of `#container` with the HTML in `markup` as XHTML.

## Compatibility

innerXHTML aims to be cross-browser compatible.  Browsers not listed here haven't been tested yet, but will likely work, too.  If you test browsers not mentioned here, let us know of your testing results.

<table id="compat">
<thead>
<tr><th>Browser</th>  <th>Platform</th> <th>Comments</th></tr>
</thead>
<tbody>
<tr><td>Camino</td>   <td>Mac</td>      <td></td></tr>
<tr><td>Firefox</td>  <td>Windows</td>  <td></td></tr>
<tr><td>Firefox</td>  <td>Mac</td>      <td></td></tr>
<tr><td>Flock</td>    <td>Windows</td>  <td></td></tr>
<tr class="warning">
    <td>IE 7</td>     <td>Windows</td>  <td>Reads unnecessary attributes for elements, such as &lt;img /&gt;.</td></tr>
<tr><td>IE 6</td>     <td>Windows</td>  <td></td></tr>
<tr class="warning">
    <td>IE 5</td>     <td>Windows</td>  <td>IE5 does not translate &lt;!--comments--&gt;.</td></tr>
<tr class="warning">
    <td>Opera</td>    <td>Windows</td>  <td>Writes uppercase element tags and attribute names. Writes unnecessary attributes, such as <em>shape="rect"</em>.</td></tr>
<tr><td>Safari</td>   <td>Mac</td>      <td></td></tr>
</tbody>
</table>

