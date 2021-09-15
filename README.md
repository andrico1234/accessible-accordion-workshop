# Create an accessible accordion

## Goals

The goal of this session is to create an accordion that:

- is operable via a range of input methods
- correctly manages focus
- is written using semantic HTML
- is resilient, due to progressive enhancement

## Terminology

WCAG - Web Content Accessibility Guidelines
Semantic HTML - HTML elements that provide information about its content to the browser.

## Divs vs Buttons

Buttons are an element that provides interactive behaviour out of the box.

- onclick
- focus management
- form integration
- disabled states
- provide additional semantic behaviour out of the box

## Tasks

Improve the initial accordion so that it satisfies the following requirements.

**Basic**

- Accordion is operable via a keyboard
- Accordion should utilise semantic mark up
- Accordion is operable via a screen reader

**Advanced**

- Accordion should function without JavaScript

### Task 1: Accordion is operable via a keyboard

Currently, our accordion can only be operated by mouse. It's important for our controls to be operable through other means, like keyboard and assistive technologies.

> End users will have varying degrees of motor skills and will benefit from keyboard accessibility. Users with motor impairment, including many elderly customers, need your help to navigate your website. Many of these users will use their keyboard to move around your website.
>
> You can find more details in the [W3C spec](https://www.w3.org/TR/WCAG20/#keyboard-operation-keyboard-operable)

To ensure compliance with WCAG, the accordion needs to be keyboard operable. This means that all functionality of a component is operable via a keyboard. Anything you can do with a mouse, you need to be able to do via a keyboard.

#### Navigate accordion via a keyboard

Currently, pressing the `tab` button doesn't move focus to the accordion headers, meaning you won't be able to operate the accordion with a keyboard. This results in content not being perceivable by keyboard users.

Go to `index.html` and change the accordion so that it becomes keyboard navigable. The most common ways of implementing keyboard navigation are:

- [tabindex](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex)
  - tabindex="0" makes the element focusable via sequential keyboard navigation
- Use [html elements](https://html.spec.whatwg.org/multipage/interaction.html#the-tabindex-attribute) that offer this behaviour by default

#### Show/hide accordion content via the `enter` key.

hedNote: Using elements like `<div>`s to implement interactive behaviour should be avoided as it requires additional work to make a `<div>` semantically equivalent with elements like `<button>` and `<input>`. Use semantic HTML when possible. If a `<button>` is focused and user presses the `enter` key, then the `click` event handler will be invoked. In other words, using semantic HTML provides expected behaviour for free.

Note: Using a `<button>` will require a few css changes.

See `solution-1.html` for a solution. Yours may be written differently, and still be accessible.

### Task 2: Accordion should utilise semantic markup

Replacing the `div` with a `button` already yields an improvement to accessibility, and the next step is to include additional semantics.

Two ways that we can achieve this is by:

1. Using the correct heading level for each section's title
2. Using aria attributes like `aria-expanded` to provide additional information to screen readers.

#### a) Use the heading element for each section's title

Using a heading element (e.g. `h2`) provides structure to your page. It also aids in screen reader navigation, [improves usability and can positiviely impact SEO](https://yoast.com/text-structure-important-seo/).

Task: Provide headings for each title in your accordion.

Hint: If you're unsure whether to write your markup like `<h2><button>Title</button></h2>` or `<button><h2>Title</h2></button>`, then try running both through an HTML validator and see which one is valid HTML. See this [SO post](https://stackoverflow.com/a/26002450) if you're really interested to find out why one is invalid html.

Note: It's a common mistake to choose a heading level based on the size of the text, than on its semantic meaning. Fortunately, CSS makes it trivial to adjust the styles to suit your branding. Look to get the markup right first, and add the styling later.

#### b) Use `aria-expanded` to indicate accordion state

The `aria-expanded` can be used to indicate whether an accordion's section is currently expanded or collapsed.

This attribute can be placed on either the content itself, or the button that controls the expanded/collapsed state. It's common practice to add `aria-expanded` to the controlling element, as adding it to the content will be redundant if the content is removed from the DOM when hidden.

This means you'll need to add the `aria-controls` attribute to the button, and pass it an idea of the element it controls.

#### c) Remove use of classes to manage element visibility

CSS Pro tip:

Instead of dedicated classes to add/remove styles, consider targeting an element's attributes.

instead of:

```css
.isExpanded {
  // styles go here
}
```

consider:

```css
[aria-expanded="true"] {
  // styles go here
}
```

By choosing to determine styles through the element's attributes, you have a single source of truth for both semantic behaviour + styling.

It also means you no longer have to manage different css classes using `containerEl.classList.add(".isExpanded")`, which could go out of sync.

Why not try and remove our calls to `currentBlock.classList.add("visible");` by using the built-in [`hidden` HTML](https://html.spec.whatwg.org/multipage/interaction.html#the-hidden-attribute) attribute.

### Task 3: Progressive Enhancement (advanced)

Progressive enhancement is the focus on building web experiences with a solid foundation, and then adding enhancements on top of that foundation. In the context of an accordion, it would mean ensuring that all content is perceivable before we add our styles and interactivity.

Since the accordion hides child content by default, and can only be displayed using JavaScript, what happens if someone tries to use our accordion, and JavaScript isn't enabled.

Note: To disabled JavaScript in your browser follow the steps:

- open up your Chrome browser console
- use cmd + shift + p to open up the command palette
- Search for "Disable JavaScript"
- press enter
- refresh the page

If you try to use the accordion, you'll find that it doesn't work.

Your task is to:
a) ensure that your accordion works with and without JavaScript
b) ensure that your markup is semantic under both scenarios

## Resources

- [Inclusive Components: Collapsible Sections](https://inclusive-components.design/collapsible-sections/)
- [ARIA authoring guides: Accordions](https://www.w3.org/TR/wai-aria-practices-1.1/examples/accordion/accordion.html)
- [HTML Spec: The tabindex attribute](https://html.spec.whatwg.org/multipage/interaction.html#the-tabindex-attribute)
- [Introduction to progressive enhancment](https://www.smashingmagazine.com/2009/04/progressive-enhancement-what-it-is-and-how-to-use-it/)
