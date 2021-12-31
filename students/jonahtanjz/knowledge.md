### Vue.js Provide / Inject

This was introduced to me while implementing submenu support for dropdown. This is useful as dropdown can have multiple levels of submenus, creating deeply nested components.

Usually, when we need to pass data from the parent to child component, we use props. Imagine the structure where you have some deeply nested components and you only need something from the parent component in the deep nested child. In this case, you still need to pass the prop down the whole component chain which might be annoying.

For such cases, we can use the provide and inject pair. Parent components can serve as dependency provider for all its children, regardless how deep the component hierarchy is. This feature works on two parts: parent component has a `provide` option to provide data and child component has an `inject` option to start using this data.

Using `provide` and `inject` allows us to more safely keep developing child components, without fear that we might change/remove something that a child component is relying on. The interface between these components remains clearly defined, just as with props.

In fact, you can think of dependency injection as sort of “long-range props”, except:
* parent components don’t need to know which descendants use the properties it provides
* child components don’t need to know where injected properties are coming from

[Official documentation of provide/inject](https://v3.vuejs.org/guide/component-provide-inject.html) 

### Event publisher / subscriber

The **publisher/subscriber** pattern is a design pattern that allows us to create powerful dynamic applications with modules that can communicate with each other without being directly dependent on each other. Similar to the **oberver pattern**, **subscribers** are like **observers** and **consumers** are like **subjects**. However, the main difference is that in the **observer pattern**, an **observer** is notified directly by its **subject**, whereas in the **publisher/subscriber** method, the **subscriber** is notified through a channel that sits in-between the **publisher** and **subscriber** that relays the messages back and forth.

[Implementation of publisher/subcriber in Javascript can be found here](https://medium.com/better-programming/the-publisher-subscriber-pattern-in-javascript-2b31b7ea075a)

This was introduced to me when I was working on the navigation menus for mobile site. I had to inform sibling components to close their overlays when a specific event occured. Passing this information to the parent and back down to the each of the sibling component will make it more verbose as well as increase coupling. By using the **publisher/subscriber**, each of the component can **subscribe** to a specific event. When a component needs to close the sibling overlays, the component can simply **publish** the event, which all **subscribers** of the event will be informed.

### Resize Observer Web API

This was discovered while working on the navigation menus for mobile. Since the navigation menus only show up on smaller screens and hide on larger screens, this means that the lower navbar can be toggled to show/hide respectively. This will cause the height of the header to change. Since there are a few **CSS** rules that use the height of the header, I had to dynamically adjust these **CSS** rules when the height changes. This also meant that I had to find a way to observe the height of the header and perform some methods when the height changes. 

The **ResizeOberver** web API reports changes to the dimensions of an `Element`'s content or border box, or the bounding box of an `SVGElement`. **ResizeObserver** avoids infinite callback loops and cyclic dependencies that are often created when resizing via a callback function. It does this by only processing elements deeper in the DOM in subsequent frames.

[Implementation details of ResizeObserver can be found here](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver)

### MarkBind's Retriever.vue for cloning reactive content

I was introduced to this specific vue component while working on the site and page navigation menus for devices with smaller screens. As the menus for the smaller screens are different vue components from the original site and page navigations, I had to pull the content from the original menus to the newly created components. I was initially searching through the DOM to find the menus, and subsequently copying these nodes, using `appendChild`, into the new menus. However, this was not the best solution as reactive content will be lost if it was duplicated in this manner. To ensure consistency of the content of the navigation menus on both desktop and mobile version, content can be cloned using the `Retriever.vue` component to ensure the reactivity of the content.

Refer to [Overlay.vue](https://github.com/MarkBind/markbind/blob/master/packages/vue-components/src/Overlay.vue) and [NestedPanel.vue](https://github.com/MarkBind/markbind/blob/master/packages/vue-components/src/panels/NestedPanel.vue) to see examples of how `Retriever.vue` can be implemented.

Internal implemenentation of `Retriever.vue` can be found [here](https://github.com/MarkBind/markbind/blob/master/packages/vue-components/src/Retriever.vue)

### HTMLElement: transitionend event

This was brought to my attention while I was working on redesigning the navigation bar. As the navigation bar takes up a fair amount of space vertically, on smaller devices, this leaves lesser space for the main content. A solution to this issue was to hide the navigation bar when the user scrolls down so that they will have more space for the main content and unhide the navigation bar when the user scrolls up so that it can be easily accessed. This was acheived by toggling the `overflow` and `max-height` of the navigation bar. To maintain a consistent transition effect, I had to make sure that the `overflow` is only toggled when the transition has completed. I was initially using `setTimeout` to match the transition duration to toggle the `overflow`. This was generally not a good way to do this as the timing of events is not guaranteed, especially across browsers or devices. To ensure that the `overflow` is only toggled when the transition has ended, we could use the `transitionend` event.

The `transitionend` event is fired in both directions - as it finishes transitioning to the transitioned state, and when it fully reverts to the default or non-transitioned state. If there is no transition delay or duration, if both are 0s or neither is declared, there is no transition, and none of the transition events are fired.

Additionally, there are also similar `transition` events, `transitionrun`, `transitionstart`, `transitioncancel`, to track the different transition states.

[Implementation details of transitionedend event can be found here](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/transitionend_event)

### Bootstrap: Display property

This was introduced to me while I was working on a PR related to the `tab` component. Prior to this PR, when a MarkBind page was printed/saved as PDF, only the active tab was displayed. The purpose of this PR was to show all the tabs when a page is being printed/saved as PDF. This requires setting certain elements of the `tab` to be displayed (the inactive tabs etc) and some to hidden (the original headers etc). I decided to use the `@media print` CSS rule to set those elements to `display: block` and `display: none` respectively.

However, MarkBind uses Bootstrap which comes with a built-in display property feature which allows users to quickly and responsively toggle the display value of components. This includes support for some of the more common values, as well as some extras for controlling display when printing. This can be done by using the display utility classes.

The classes are named using the format:
* `.d-{value}` for xs
* `.d-{breakpoint}-{value}` for sm, md, lg, and xl.

For the PR's use-case, elements can be assigned the classes `.d-print-{value}` to change the display value of elements when printing. For instance, `.d-print-none` can be used to hide elements when printing and `.d-print-block` can be used to show elements when printing. Classes can be combined for various effects as you need.

More examples:
```html {.no-line-numbers}
<div class="d-print-none">Screen Only (Hide on print only)</div>
<div class="d-none d-print-block">Print Only (Hide on screen only)</div>
<div class="d-none d-lg-block d-print-block">Hide up to large on screen, but always show on print</div>
```

[Full implementation details of display property can be found here](https://getbootstrap.com/docs/4.0/utilities/display/)

### Lodash: `_.differenceWith` method

This was discovered when I was working on a PR related to the live preview of the `pages` attribute in `site.json`. I needed to check if the `pages` attribute has been modified, and if so, which are the modified pages (including pages that are added and removed). Since the `pages` attribute is read in as an array of objects, I needed a way to check each individual object to see if one or more of their properties have changed. I was initially aware of lodash's `_.difference` method which returns an array of array values not included in the other given arrays. However, this is not sufficient to detect changes to specific properties of an object. After looking around, I found another lodash's method, `_.differenceWith`, which is similar to  `_.difference` except that it accepts comparator which is invoked to compare elements of array to another array. With the comparator, this method can now look for changes to specific properties of objects.

For instance, the below code block will show how `_.differenceWith` can be used to track the added and removed pages using a single comparator, `isNewPage`.
```js {.no-line-numbers}
const isNewPage = (newPage, oldPage) => _.isEqual(newPage, oldPage) || newPage.src === oldPage.src;

const addedPages = _.differenceWith(this.addressablePages, oldAddressablePages, isNewPage);
const removedPages = _.differenceWith(oldAddressablePages, this.addressablePages, isNewPage)
```

Another example to show how `_.differenceWith` can be used with a more specific comparator to find edited pages.
```js {.no-line-numbers}
const editedPages = _.differenceWith(this.addressablePages, oldAddressablePages, (newPage, oldPage) => {
    if (!_.isEqual(newPage, oldPage)) {
        return !oldPagesSrc.includes(newPage.src);
    }
    return true;
});
```

With this method, I am able to quickly identify differences in any objects in an array. This method is also flexible as I can declare my own comparator.

Current implementation of `_.differenceWith` can be found in the [`handlePageReload`](https://github.com/MarkBind/markbind/blob/master/packages/core/src/Site/index.js#L867) method in `index.js` of `Site`.

[Full documentation of Lodash's `_.differenceWith` can be found here](https://lodash.com/docs/#differenceWith)
