### Vue Lifecycle Hooks

I learnt about Vue's lifecycle hooks while working on popovers as I was facing errors with undefined instance properties. 
For instance, the `$el` property is only resolved after the instance is mounted.
When working with instance properties such as `$el`, I also learnt more about the different lifecycle hooks and when different properties are instantiated.

I came across this important note while browsing the [docs on lifecycle hooks](https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks):

> Don’t use **arrow functions** on an options property or callback, such as `created: () => console.log(this.a)` or `vm.$watch('a', newValue => this.myMethod())`. Since an arrow function doesn’t have a this, this will be treated as any other variable and lexically looked up through parent scopes until found, often resulting in errors such as `Uncaught TypeError: Cannot read property of undefined` or `Uncaught TypeError: this.myMethod is not a function`.

Resources
* [Vue lifecycle hooks](https://www.digitalocean.com/community/tutorials/vuejs-component-lifecycle)
* [Vue lifecycle diagram](https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram)

### Vue Templates & Slots 

Templates/slots are a way to pass content from parent to child components, and are used by bootstrap-vue (in place of props) in cases that require reactivty.
Using slots could make the code more readable. 
It's also a convenient way to pass components into another component, and enables reactivity in the popover use case.

Resources
* [Vue slots](https://vuejs.org/v2/guide/components-slots.html)

### Data attributes

I came across the use of data attributes (`data-*`) to pass data between different components/nodes in MarkBind.
I thought it was an interesting feature of HTML that allows for extensibility as users can store extra properties on the DOM without interfering with standard attributes.
It also guards against future changes in standard HTML attributes (i.e., avoids the problem of using a non-existent attribute that may become a standard attribute in a future version of HTML).
This was something that I was previously not aware of so it was interesting to see how it could be used in a practical scenario.

Resources
* [Using data attributes](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)

