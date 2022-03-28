### Special characters encoding on URI

On windows, file paths are separated by the directory separator character which
is a `\`. This is different from the linux and mac's `/` separator. When
building tools to be used cross-platform, it is important to be able to parse
and encode file paths correctly. This is especially important when dealing with
URI since they only accept 84 characters with `\` not being one of them. Hence,
if a URI contains a `\` character, it will be encoded as `%5C` and may cause
issues with the path on a webpage. Read more about this
[here](https://stackoverflow.com/questions/1547899/which-characters-make-a-url-invalid).

### HTML Canvas drawing

I used HTML canvas to help with image annotation. I choose to use HTML canvas
since it provided the best performance and I felt it was well suited to the task
of drawing arrows and annotating over images.

Below are some things that I found useful while working with HTML canvas.

The HTML `<canvas>` element is used to draw graphics, on the fly, via
JavaScript. The `<canvas>` element is only a container for graphics but the
actual drawing of the graphics is done using javascript by the user. I made use
of lines, boxes, text and image to help annotate over images. Once an image/line
is drawn, there is no way to erase that line unless the whole canvas is wiped
out (or draw a white box over that section). Due to this feature of canvas, I
had to parse all the image data and set the proper width, height and coordinates
directly before actually beginning to draw the canvas element. This made drawing
individual annotations harder since I had to account for the starting position
of each element and make sure that they are all in line.

I also found that resizing the canvas width or height will cause the whole
canvas to be reset to a blank slate. This was one of the bigger issues I ran
into since now I had to pre-calculate the final size of the canvas.

Resources:

- [html-canvas Guide](https://www.w3schools.com/html/html5_canvas.asp)
- [html-canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)

### DIVs vs SVG vs HTML Canvas for drawing

#### Advantages

DIVs are simple to work with and are the most common components for frontend
developers. They are quite user friendly and are good for simple animations and
drawings.

SVGs scale well and are a good choice for complex drawing. They are also
relatively fast compared to DIVs and are computed in a way that there is no loss
of quality at any scale.

Canvas is a good choice for lines, boxes and text drawing. It is the fastest of
the 3 methods and can be used to draw over an existing image. This functionality
makes it a good choice for annotating over any given image.

#### Disadvantages

DIVs did not offer the flexibility of SVG and HTML canvas, especially for
drawing more complex shapes like arrowheads. Positioning of div elements also
tends to be clunky as I would have to work with CSS positioning as well. This
might be an issue when scaling is involved since relative position tends to
change significantly depending on the size of the screen.

SVGs were complicated to overlay over an image and creating many different
custom SVGs would require a use of a external library (or very complicated
code). SVGs would also have to be referenced for each rended object and this
would be troublesome since I am passing child components to the parent (making
references difficult due to the similar names).

HTML canvas is complicated to work with and requires a lot of javascript. It
also requires the image to be drawn in one go (depending on application) as
often times drawing over a already drawn image does not preserve quality well.

Resources:

- [Performance difference between DIVs, SVG and HTML Canvas](https://stackoverflow.com/questions/5882716/html5-canvas-vs-svg-vs-div#:~:text=The%20short%20answer%3A&text=SVG%20objects%20are%20DOM%20objects,yourself%2C%20or%20use%20a%20library.)

### Attributes and Query Selector

The `querySelector` method is used to find elements in the DOM. I used it to get
the info of all child classes from the `annotation-wrapper`. This allowed me to
get the attribute data needed to build the html canvass before drawing. I choose
to use `querySelector` over emitters due to the way my components where
structured. By using `querySelector` adding more annotation features to the
class will be relatively easy since all I have to do is add another method to
query the new element id.

Resources:

- [query selector api](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelector)

### Vue Slots

### Image Scaling and Cropping
