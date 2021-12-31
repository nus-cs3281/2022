### `htmlparser2`: DOM Traversal, Manipulation, and Things to Watch Out

MarkBind uses a combination of [`htmlparser2`](https://github.com/fb55/htmlparser2) and [`cheerio`](https://cheerio.js.org/)
for operations with the HTML representation before finalizing the page to be served, the former is for parsing HTML strings
into pseudo-DOM nodes, and the latter is for manipulating those nodes, adding/removing new nodes, and selecting specific nodes,
akin to [`jQuery`](https://jquery.com/).

However, as `cheerio` does a lot of work behind the scenes (especially in `cheerio.load`), the library can be deemed as expensive to use in a
performance-sensitive environment. Thus, `cheerio` is used sparingly, and when the situation calls for it. With this in mind,
I had to be creative in manipulating the DOM as best as I can without `cheerio`.

After some research and manual look into the DOM nodes in MarkBind, I get some understanding of the DOM nodes are structured. \
A DOM node provided by `htmlparser2` is generally structured like this:

```js
{
    type: "tag",
    name: "span",
    attribs: { class: "no-lang" }
    children: [
        {
            type: "text",
            data: "This is a span",
            children: [],
            parent: ..., // Circular reference to the "tag" node
            prev: null,
            next: null
        }
    ],
    parent: null,
    prev: null,
    next: null
}
```

Where each properties are:
- `type`: The type of the node, it can be `"tag"` for an element with HTML-tags, `"text"` for just a plain-text, and more.
(Note that `"text"` nodes cannot exist alone, it must be a child of another node)
- `name`: In the case of `"tag"` nodes, this is the name of the tag, such as `"span"`, `"div"`, etc.
- `attribs`: In the case of `"tag"` nodes, this is an object to store attribute data of the tag, such as classes, ids, etc.
- `data`: In the case of `"text"` nodes, this is the actual text content.
- `children`: An array of DOM nodes which describes the node's children.
- `parent`: The DOM node for the node's parent.
- `prev`: The DOM node preceding this node.
- `next`: The DOM node right after this node.

Once I understood this structure, I can start to play around with the DOM nodes.

First thing is to try to do a traversal of the node. Generally, we can do a depth-first traversal by
recursing to its `children`. I found it very useful if you want to process continuous data such as texts with markups inside it.

Then manipulation is as simple as modifying the properties of the DOM nodes.
- If you want to add new classes, you can just append the `attribs.class` property with the new class.
- If you want to reorder the children, you can directly change the `children` array.
- In a similar vein, if you want to add or remove child nodes, you can directly add or remove it on the `children` array.

However, when you play around with the actual structure of the node (like the last two above), we need to watch out several
things on the references.
- Set the `prev` value to be the node that is directly before this.
- Set the `next` value to be the node that is directly after this.
- After the two is finished, update the node's `parent` to the node's actual parent.

This should address the referencing pretty well for the node to be rendered properly. However, I have to note that the
references are better to be handled with `cheerio` whenever possible, and direct manipulation of references are to be done
when under a constraint.

### `highlight.js`: Tokens and HTML-Preserving Quirks

In researching ways to partially highlight text in a syntax-highlighted code block, I ended up understanding part of the way
[`highlight.js`](https://highlightjs.org/) applies syntax highlighting to a text.

`highlight.js` breaks down the text to *tokens*, where each token will be wrapped as a `<span>` and has a specific styling
prepared in the applied theme's CSS. These tokens may actually contain other tokens, in which it will be displayed as nested
elements. The token names are useful if you want to target specific styling to specific types of tokens, such as adding stronger
color for variables, muting down the color for comments, and so on. 

Then there is a quirk to `highlight.js` highlighting method that it somehow preserves HTML in a code block when the text is
being broken down to tokens. That is, the HTML code does not get parsed and are not considered a part of the code block. That
means, one can add a custom styling HTML element wrapping a part of the text and the styling will still be displayed even though
the element was parsed and styled by `highlight.js`.

This was a good candidate for partial text highlighting. But unfortunately, this does not apply when the language being parsed
is HTML and other variants (e.g. XML). So, partial text highlighting cannot be reliably done in this way across all languages.

References:
- https://github.com/highlightjs/highlight.js/issues/1561

### `live-server`: The Live-reload Mechanism and Things to Know

MarkBind uses [`live-server`](https://github.com/tapio/live-server) to provide live-reload capabilities for users. In essence, given a configuration object, live-
server starts a local development server, opens a browser tab that loads a specified file, sets up the live-reload mechanism for each page that will be opened, and listens to changes in the watched folders and issue reloads to opened tabs whenever it detects one.

During the development of a solution to , deeper research into how `live-server` does the 

#### How does `live-server` provide the live-reload mechanism? 

The live-reload mechanism is done by the use of [WebSockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API), which is a protocol that provides two-way communication over a single TCP connection. `live-server` leverages
this as a channel between the package and the opened pages. Messages are sent through this WebSocket informing
to reload the page whenever the package wants to issue a reload.

To establish the WebSocket connection and to actually reload the page, it needs a script instructing the window to do so.
`live-server` does this by injecting an inline `<script>` to the file before sending a response to an HTML (or similar)
files, such that this script will be run during the page load. The package is able to inject the script midway through the
request-response pipeline as the package has full control of the server handling.

When the script starts to establish a WebSocket client-side, it sends a request to the server with the WebSocket protocol.
The server receives this and sends a 101 Switching Protocols response, of which the package attaches a listener to and
complete the establishment phase by creating the corresponding WebSocket server-side and adding it to the package's local
variable that holds the current active sockets. Both WebSocket instances are coupled and make a communication channel
for that page. Whenever the page is closed, the WebSockets emit a `close` event which is handled by the package to remove
the WebSocket from the local storage variable.

When a change is detected on the folder specified in the initial configuration file, the package will iterate over the
active WebSockets stored on the local storage variable and sends the message to client-side to reload the window for each
WebSocket.

In essence, the establishment flow of live-reload can be summed up to:

1. Request-response phase: `Receive request` > `Inject live-reload script to response` > `Send response`, then
2. Document-load phase: `Load document` > `Execute live-reload script` > `Establish client-side socket`
3. Socket-initialisation phase: `Receive socket request` > `Establish server-side socket` > `Send connected message`

Note that the live-reload mechanism is set up *after* request-response phase finishes.

#### Things to Know about Live-reload

In the implementation of `live-server` there are some quirks about the live-reload mechanism, some of which is instrumental
in figuring out the behaviour of the live-reload, and some I end up addressing on during the development of multiple-tab
development for MarkBind.

1. During reload, the original socket is closed and a new one reopens.

A socket only lives until users navigate to other pages within that tab, or the tab reloads. When the tab reloads, instead
of retaining the same socket as assumed, the socket will close first, and a new one will open. This behaviour becomes clearer
after understanding that on reload, the window loads again, which goes back to the Document-load phase.

2. Reloads will be issued on *any* changes in the watched folder, not just related ones.

The implementation in `live-server` issues reloads indiscriminately on file changes. This can certainly cause user experience
issues if the watched folder is volatile, we don't want the users to experience reloads every second or so. A stronger guard
is required to remedy this, such that the reloads are only issued when the file that is changed corresponds to any URL that
is currently opened.

3. There are no stored knowledge of the socket correspondences.

The package only stores the sockets without any additional data of which URL this socket corresponds to. This makes
differentiating sockets harder when we want to do a running status of the URLs are currently opened through the server.

### JavaScript: How to make a series with asynchronous callbacks runs sequentially

When developing multiple-tab development for MarkBind, there is a need for page rebuilds to prioritize pages that are
currently opened, and of which is rebuilt in the order of most-to-least recently opened. While page generation is run
asynchronously, this raises a need for a series of asynchronous callbacks to run in sequential.

One might come to a solution of using an `Array.forEach`, assuming that async callbacks are executed one by one. However,
note the implementation of the function:

```js
Array.prototype.forEach = function (callback) {
    for (let index = 0; index < this.length; index++) {
        callback(this[index], index, this);
    }
};
```

On each of the iteration, the function executes the callback and goes to the next iteration. For sequential callbacks this
essentially goes one-by-one as hoped, but for asynchronous functions, it applies all the callbacks but it does not wait for it
to be done in each iteration, so the execution order is not guaranteed.

Surprisingly, there are no built-in functions to do what we want. But fortunately, the solution is very simple: add an `await`
on the callback call:

```js
function sequentialAsyncForEach(arr, callback) {
    for (let index = 0; index < this.length; index++) {
        await callback(this[index], index, this);
    }
}
```

References:
- https://codeburst.io/javascript-async-await-with-foreach-b6ba62bbf404

### WebStorm IDE: Save Resource by Unmarking Compiled Folders

When developing the background-build system for MarkBind and testing it towards the documentation files, I noticed that the CPU
usage goes high when WebStorm is in its indexing phase, which can go up to very high percentages (90-100%). The indexing
happens as there are changes in the project folder. As I was testing it against the files which are within the project folder,
the folder that stores the compiled files are also within the project folder, and as it keeps getting updated, WebStorm keeps
doing indexing which can tax the user's machine unnecessarily.

My finding and recommendation is to unmark this compiled folder, such that WebStorm does not trigger its indexing phase, and
save resources in doing so.

References:
- https://www.jetbrains.com/help/webstorm/configuring-project-structure.html

### `lodash`: Performance of `uniq` vs JavaScript's Set

For the case of filtering out elements and later testing some other elements against the former, we have two possible options:
using `lodash`'s `uniq` and test the elements with `Array.some`, or use the native `Set` and test against `Set.has`.

General experience might suggest the `Set` option as existence checks is pretty much done on constant time as compared to
`Array.some` which is linear in the size of the array. However, the performance test done on `uniq` vs `Set` shows that even
with small items the former can be up to 3 times faster. This speed difference renders the advantages of `Set.has` not really
worth it.

With this finding, I am more inclined to use `uniq` when I know the filtered result is small enough (or I know the bounds of
the array is small), otherwise I will use `Set` in general.

References:
- http://measurethat.net/Benchmarks/Show/3889/0/lodash-uniq-vs-set