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

### Arcsecond

[Arcsecond](https://github.com/francisrstokes/arcsecond) is a zero-dependency parser combinator library for Javascript that is now being used in CATcher to parse GitGub issues and comments. 

Previously in order to parse the  comments, we used the regex string `(${headerString})\\s+([\\s\\S]*?)(?=${headerString}|$)\gi` which is neither human readable nor maintainable. In addition, this string only finds matches - more regex is used to extract relevant information from the comments. 

In comparison, arcsecond offers human friendly parsers such as `str`, `char`, `letters`, `digits`, `between` and so on, and these parsers can be composed and run in sequence. This makes building and maintaining parsers much easier. In addition, arcsecond also allows you to extract certain information from a string (as opposed to the entire string) by way of the `coroutine` parser. 

For example, take the string "Today is Wednesday and the weather is 30 degrees Celsius", and you want to extract the day (Wednesday) and the temperature (30). One parser that can achieve that is:

```typescript
const dayWeatherParser = coroutine(function*() {
  yield str("Today is "); // parse and ignore
  const day = yield letters; // parse 'Wednesday' and store
  yield str(" and the weather is "); // parse and ignore
  const temperature = yield digits; // parse '30' and store
  yield str(" degrees Celsius"); // parse and ignore

  return {
    day: day,
    temperature: temperature
  }
})
```

This allows us to build complex and versatile parsers easily, yet in a way that is clear and understandable. For more information, check out the [tutorial here](https://github.com/francisrstokes/arcsecond/blob/master/tutorial/tutorial-part-1.md)

### Jasmine

[Jasmine](https://jasmine.github.io/) is a testing framework for Javascript code. In Jasmine, test suites are functions in `describe` blocks, and each spec is also a function in an `it` block. 

For example, here is a suite that tests a certain function:

```typescript
function testMethod() {
  return true;
}

describe("testMethod suite", () => {
  it("testMethod should return true", () => {
    expect(testMethod()).toBe(true);
  });
});
```

Expectations are built with the function `expect` which takes a value (`testMethod()` in the example above), and is chained with a [Matcher](https://jasmine.github.io/api/edge/matchers.html) such as `toBe`, `toEqual` or `toBeGreaterThan`. This provides greater flexibility than say JUnit's assert methods, since one assert method corresponds to one condition.

Since test suites and specs are functions, normal Javascript scoping rules apply, so variables can be shared between specs. In addition, there are separate setup and teardown methods such as `beforeEach` and `afterAll`.

For more information, check out the [Your First Suite tutorial here](https://jasmine.github.io/tutorials/your_first_suite)
