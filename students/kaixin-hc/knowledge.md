(in progress)

### Vue

List the aspects you learned, and the resources you used to learn them, and a brief summary of each resource.

* https://v1.vuejs.org/guide/instance.html
### Scrollbars

I was curious about the scrollbar issue, so I did a little digging for my own learning. After a few stack overflow posts and reading, I found this article: https://css-tricks.com/popping-hidden-overflow/ that explains that setting overflow-x sets overflow-y as well, and it's just not possible to have a dropdown peep out of a scrollable overflow without setting positions relatively. This discussion with the offered solution was also interesting. Not sure how to implement it cleanly in a vue component, but it is a non-issue.

I think it also shows the need for #1714
### Logging Framework

## Open source dependencies

### ghpages

### Commander

### CI pipeline (particularly with git)

...

### Ways Versioning is Implemented
...
### Javascript with regard to object oriented programming

Looking into this was inspired by the issues on refactoring the large `core/site/index.js` file which is over 1.5k lines into more manageable class. At present, most of the file is made up of the `Site` class, which makes sense from an object oriented perspective. All the functions which are supported by Site are things which affect what the site itself holds or does: generating itself, deploying itself, initialising itself.

One suggestion for refactoring would be separating out each command into separate files. We could abstract away the command logic might be separating each command into classes, having each command inherit from a Command class, and having the site class just generate and execute each command when it is called to do so. But is this necessary or desirable?

Java and Javascript are different in that Java is class based and Javascript is prototype-based. Class based languages are founded on the concept of classes and instances being distinct, where classes are an abstract description of a set of potential instances which have the properties defined in the class - no more and no less. Prototype based languages have a 'prototypical object' which is the template used to create a new object, but once you create it or at run time the object can specify its own additional properties and be assigned as the prototype for additional objects (source: [mozilla, class-based vs prototype based languages](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Details_of_the_Object_Model))
