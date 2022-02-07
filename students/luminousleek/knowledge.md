### CSS Flexbox

Flexbox is used to order, align and space out items in a one dimensional container (i.e. a row or a column), even when the size of the items are unknown or dynamic. 

Items can be given a default size (`flex-basis`), and can also grow (`flex-grow`) and shrink (`flex-shrink`) according to the extra space in the container (or lack thereof). These three properties can be controlled with the `flex` property, e.g.

```css
.item {
    flex: 0 1 10%
}
```

where the three parameters correspond to `flex-grow`, `flex-shrink` and `flex-basis` respectively. In this case, the default width of the item is 10% of the width of the container, and it does not grow nor shrink relative to the other items (assuming the other items also have their `flex` property set to `0 1 auto`).

The `flex-basis` property can also be set to the `content` keyword, where the width of the item is based on the content within the item (e.g. if it contains text, then the width of the item is the length of the text string). This allows for dynamically sized items within the container, which may enable the layout to look cleaner.

For a helpful summary of flexbox properties, visit [https://css-tricks.com/snippets/css/a-guide-to-flexbox/](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

### Prettier, husky and pretty-quick

Prettier is a tool to format code according to a given code style automatically. Unlike a linter such as TSLint, prettier only cares about formatting, rather than detecting programming errors. Prettier supports both Typescript and Angular, which CATcher is written in.

Since it is quite wasteful to run prettier to format the entire codebase every time a change is made, so a more efficient method is to format the codebase once, and then format only the changes made during each commit. 

This is where the husky tool comes in, which enables hooks to be run before each commit. The relevant hook here is pretty-quick, and this formats the changed/staged files during each commit. This frees developers from having to fuss with maintaining code formatting or fixing formatting mistakes, leading to less frustration. 

For more information, visit [Prettier](https://prettier.io) and [husky](https://typicode.github.io/husky/)