# What is innerXHTML?

innerXHTML is a glorified DOM read/writer. It is my answer to the non-standard innerHTML, the result of too many late nights in front of the computer screen and the aspirin to many of my DOM scripting headaches. It utilises existing functionality, all supported by the W3C, to read and write to the DOM. In doing so, innerXHTML gains all the benefits of innerHTML without the baggage; speed and ease of use, whilst keeping within the boundaries of global standardisation.

innerXHTML is not a document copier; it does not fetch exactly what the document contains, character for character in a zombie-like fashion. innerXHTML reads the document node tree, returning a text string of markup to the document's equivelent. Likewise, innerXHTML writes to the DOM with the correct approach; intelegently, node by node, not as one big string. You wont find any `document.write()` here!

# How does innerXHTML work?

It is split into two functions. The first function innerXHTML acts as the interface between the two. It reads the document node tree and returns markup in plain text to the source that called it. The second function translateXHTML serves the purpose of translating XHTML and XML code from a plain text string into nodes for insertion into the document object model. translateXHTML is the more processor-heavy function and is additionally more complex in structure. You can operate translateXHTML independently from innerXHTML, though I don't recommend this.

# Using innerXHTML to read from the DOM

I have built innerXHTML to work much like innerHTML. Once the .js file is referenced by your markup document using a `<script src="">` you can use innerXHTML like any other function:

    var container = document.getElementById('container');
    var code = innerXHTML(container);

This snippet of script above, will find the node with an id attribute of 'container' and, using the innerXHTML function return it's entire content in a plain text string - markup 'n all. The one and only parenthesis is the mandatory source element from which we are reading. Remember, innerXHTML will return the markup of whatever nodes the source element contains. Data returned will not include the source element's own tag and attributes, mirroring the non-standard innerHTML. Lets take a quick look at the aforementioned innerHTML for execution comparison:

    var container = document.getElementById('container');
    var code = container.innerHTML;

As you can see innerXHTML takes a slightly different approach, but ultimately remains just as easy to use.

# Using innerXHTML to write to the DOM

Whilst reading has its benefits, innerXHTML really comes into its own over regular, hand-written document.createElement() techniques when writing to the DOM. Traditionally every element, and sub-element, and sub-sub-element has to be individually created by you the developer, in addition to each element's potentially multiple attributes and contained text nodes. This is a labouring process to say the least.

Using innerXHTML all you are required to provide is the plain string of markup. The function will do the hard work of reading and translating the markup into DOM nodes and inserting them into your document for you. Let's take a look:

    var container = document.getElementById('container');
    var markup = '<div id="content"><p class="text">Hello world!</p></div>';
    var code = innerXHTML(container,markup);

Our innerXHTML function accepts two parentheses this time; the mandatory source element in which we are writing, and secondly the string of text markup to replace any existing content therein. The above code fragment finds the element with an id of 'container' and, via innerXHTML, inserts into it a `<div>` with an id attribute of 'content'. Into that it appends a `<p>` paragraph element with a child text node of 'Hello World!'. Don't worry about any document.createElement(); messing about; innerXHTML takes care of this all for you. The variable code here is optional and will now contain exactly the same text markup as you put into the function.

Some people use innerHTML for the purpose of quickly emptying an element of its children. This is a perfectly good use of innerXHTML with a command such as `innerXHTML(container,' ');`. Just be aware that a value must be passed for the second parenthesis; ideally, a space. Technically this means you are not emptying the source element completely, but rather replacing its content with a single space text node. However for practical use this should serve the purpose.

# Additional Information

I have repeatedly streamlined innerXHTML to run as efficiently as possible, and from earlier (you could call them alpha ;) ), development versions the results do show. innerXHTML is quick in operation and can construct full pages from markup easily, depending on the computer and browser speed. It is naturally not as fast as the non-standard innerHTML, but the differences are in miliseconds, again depending on the quantity of code. Speed tests coming soon.

Please remember that this function expects only code adhering to web standards. Passing the function non-standard markup or trying to read an invalid document will likely result in process failure!

Additionally I have taken time to ensure innerXHTML is as cross-browser compatible as possible, hence some structural changes along the development line. Below is a table of browser support information. Please note that the browsers listed below are the only ones that I have tested innerXHTML on. If a browser you are looking for is not listed here then it will not mean that innerXHTML does not run, it simply means I have not tested it as yet. If you do undertake any further testing and have feedback then please get in touch with me. It's all good :)

<table title="innerXHTML Compatibility Statistics" class="information_table">
  <thead>
    <tr>
      <th>Browser</th>
      <th>Supported?</th>
      <th>Additional Comments</th>
    </tr>

  </thead>
  <tbody>
    <tr>
      <th><abbr title="Internet Explorer">IE</abbr>7 Windows</th>
      <th>Full support <small>(Minor proprietary characteristics)</small></th>
      <td>Some elements, such as &lt;img /&gt;, are given extra attributes when read with innerXHTML. However, further testing has found this causes no fracture to the functionality and the differences are likely to go unnoticed.</td>

    </tr>
    <tr class="row_even">
      <th><abbr title="Internet Explorer">IE</abbr>6 Windows</th>
      <th>Full support <small>(Minor proprietary characteristics)</small></th>
      <td><span class="line-through">'on' events <small>(onclick, onmouseover, etc.)</small> do not work when translated from text into document.</span><a href="#release0-4">Fixed in v0.4+</a>.<br>See IE7 (above).</td>

    </tr>
    <tr class="row_even">
      <th><abbr title="Internet Explorer">IE</abbr>5 Windows</th>
      <th>Virtually full support</th>
      <td>In addition to the IE6/7 proprietary characteristics (detailed above) IE5 does not translate &lt;!--comments--&gt;. However markup created on-the-fly with <abbr title="Javascript">JS</abbr> does not appear in the source document anyway, so unless you plan to insert markup then read it back into a string, you're not going to notice a thing :)</td>

    </tr>
    <tr>
      <th><abbr title="FireFox">FF</abbr> Windows</th>
      <th>Full support</th>
      <td></td>
    </tr>
    <tr>

      <th>Flock Windows</th>
      <th>Full support</th>
      <td></td>
    </tr>
    <tr class="row_even">
      <th><abbr title="FireFox">FF</abbr> Mac</th>

      <th>Full support</th>
      <td></td>
    </tr>
    <tr>
      <th>Safari Mac</th>
      <th>Full support</th>
      <td></td>

    </tr>
    <tr class="row_even">
      <th>Camino Mac</th>
      <th>Full support</th>
      <td></td>
    </tr>
    <tr>
      <th>Opera Windows</th>

      <th>Full support <small>(Minor proprietary characteristics)</small></th>
      <td>Writes elements using uppercased tag and attribute names, and enters unnecessary attributes such as <em>shape="rect"</em>. However, further testing has found this causes no fracture to the functionality and the differences are likely to go unnoticed..</td>
    </tr>
  </tbody>
</table>

Important: innerXHTML translates and inserts XML into the DOM without a problem in all browsers. It reads and returns XML correctly in most browsers too; every one except Microsoft Internet Explorer 5&6. This is entirely down to poor DOM support by IE. For the same reason IE 5&6 will not read any part of the document node tree containing element(s) it does not recognise. In practical use this basically means by example that innerXHTML will not read any part of the document tree containing the `<abbr>` element in Internet Explorer 6 or less. Internet Explorer 7 has corrected the problem.


## Update: 21/03/2007 - Version 0.4 Released

Almost 6 months following it's release innerXHTML has transcended to version 0.4. The updates make the function slicker, easier and more cross-browser compatible than ever. They are:

### `$appendage` argument added

The function now accepts three arguments rather than just two. The third argument is $appendage and can dictate where in the parent node you place your inserted markup string. Let's use this document slice as the basis for our examples:

    <div id="content">
      <h2>About Google</h2>
      <p>Google was co-founded by Larry Page and Sergey Brin while they were students at Stanford University.</p>
      <p id="closing_paragraph">The name "Google" originated from a misspelling of "googol", which refers to 10<sup>100</sup> (the number represented by a 1 followed by one hundred zeros).</p>
    </div>

### innerXHTML's third argument now accepts four value types:

1. Default boolean false: Empties the parent container.
2. Child node: Enter a child node and the markup string you pass will be inserted before it.
3. String ID: As above, except the child is located using the ID of the string you pass.
4. Any other true value: Append the string markup you pass after existing parent's children.

Respective examples:

    // This will empty the DIV with a id of 'content'  before inserting the link
    innerXHTML(document.getElementById('content'), '<a href="#">click me</a>');

    // This will insert the link before the first paragraph
    innerXHTML(document.getElementById('content'), '<a href="#">click me</a>', document.getElementsByTagName('p')[0]);

    // This will insert the link before the second paragraph
    innerXHTML(document.getElementById('content'), '<a href="#">click me</a>', 'closing_paragraph');

    // This will append the link into the DIV after the existing content
    innerXHTML(document.getElementById('content'), '<a href="#">click me</a>', true);

### First $source argument now accepts string ID

Speed things up by entering the parent id as the first argument, rather than a node reference. Using the last snippet as an example:

    innerXHTML('content', '<a href="#">click me</a>');

### 'on' events now supported by all browsers

Does what it says on the tin: Internet Explorers now also support inserted on events. Example:

    innerXHTML('content', '<a href="#" onclick="alert(\'Hello World\');">click me</a>');


I'd like to thank Stef Dawson for his most appreciated contributions and feedback. He's made the 'on' event patch in IE a reality - cheers Stef :).

If anyone else has any ideas for improving innerXHTML then don't hesitate to let me know. Let's put an end to this non-standard bollocks once and for all.
