# CSS

#### Using Calc() to make 100vw images, which remain inside container

```css
img {
  display: block;
  max-width: 100%:
}

.big-image {
  max-width: 100vw;
  width: 100vw;
  margin: 0 calc(-50vw + 50%);
  max-height: 30vh;
  object-fit: cover;  /* to make the image not squish */
  /* 0 on the top and bottom*/
  /* -50vw pushes the image to the left */
  /* +50% pushes it to the right, adding half of its own width as margin */
}
```

---

#### Viewport units use the space below the scroll bar in calculations

so we can do

```css
body {
	overflow-x: hidden;
}
```

---

#### `em` vs `rem`

for `font-size`, `em` looks at the parent's font-size, bubbling up to `body` if font-size on parents are not defined.

for `margin` & `padding`, `em` looks at the font-size of the element, (not the parent).

`rem` always looks at the font size of the root element `html`

---

## Default Margins

- `<p>` tag gets a default margin-top/margin-bottom of the font-size

## Margin Collapsing

- By default, margins collapse into one another

- So, if one element has `margin-bottom` of `50px`, and the next one has `margin-top` of `50px`, the space between them will be `50px` and not `100px`

- Margins will collapse any time they touch each other, so, if the first child in an element has a margin-top, that can merge with the parent's margin top. (This is the reason we see `body` moving "downward" when the first element, usually the `h1`, is given a margin-top.)

- Example

  ```html
  <div class="card">
  	<h1 class="main-heading">I am the main heading</h1>
  </div>
  ```

  ```css
  .card {
  }

  .main-heading {
  	margin-top: 100px;
  }
  ```

  The above `100px` margin merges with margin of `.card` (even though it does not have a margin at all !!!), and the whole card will have a `margin-top` of `100px`. This is because `h1` margin top is touching the margin of `.card`.

  The solution is to give the parent a `padding-top`.

NOTE: Margin collapsing does not happen with flex-box

## Inline Elements

- `<span>` `<em>` `<strong>`

- Follow these rules
  - Can't put a block element inside one
  - setting width, height, margin-top and margin-bottom has no effect.
  - setting padding-top, padding-bottom just "overrides" the neighboring elements, as if the other elements are ignoring the padding.
  - So, they only respect margin, padding and border which are placed on the left or the right side, and not the top or bottom.

## Margin and Padding

- For consistency, we often turn-off margin-top on typography related elements.

- we can do that with one selector, using comma separated list

```css
h1,
h2,
h3,
p {
	margin-top: 0;
}
```

- so, to have space on top and bottom of the content, add `padding: y px` on the parent.

- Related best practice to the above is to have the last element on the page have a `margin-bottom: 0`, so that the padding & margin dont clash at the bottom of the page.

## Block vs inline-block elements

- Block level elements create a new line of content, stacking on top of each other e.g. `<p>, <header> <footer> <main> <section> <li> <ul> <div> <h1>`
- Block level elements have a width of 100% of their parent by default.

- Inline elements stay within the flow of what's around them. e.g. `<a> <b> <em>`

  - usually placed inside block elements

  - They only respect margin, padding and borders which are placed on the left or the right side, and not on the top or bottom

  - cannot set width and height on them!

## Styling Links

- Links have several states

  - Default "link" state
  - Visited
  - Focus
  - Hover
  - Active (While the link is being clicked)

- These states need to be in the order as shown above, otherwise due to specificity some states maybe overwritten by others.

## Styling buttons

- Use Padding to give "width" and "height" to buttons.
- Always put the class on the button/link, rather than the container.
- Use a ratio of 1:2.5 (h:w) on a button.

## Media Queries

- Media queries lets us add new styles that only target specific conditions

- syntax `@media () { ... }`

```css
@media media-type and (media-features) {
	// ...
}
```

- **Media type** lets us target different types of media
  - Screen
  - Print
  - Speech
- **Media Condition** lets us target specific conditions within the **media type**
  - Widths
  - Orientation
  - Specific features
- both these properties are optional, but at least one must be specified.
- we can combine type with a condition using `and` keyword

```css
@media screen and (min-width: 960px) {
	// ...
}
```

- Examples

```css
/* works over and above 500px, not below */
@media (min-width: 500px) {
	body {
		background: pink;
	}
}

/* works over 501 px but only up to 999px */
@media (min-width: 501px) and (max-width: 999px) {
	body {
		background: yellow;
	}
}

/* works up tp 1500px and not more than that */
@media (max-width: 1500px) {
	body {
		background: orange;
	}
}
```

*** 

## CSS Grid 

### auto-fit vs auto-fill

- auto-fit will fit the columns you've defined into the available space (it's a bit more complicated in spec)
- auto-fill will keep adding columns, even if they are empty


***

## CSS Position

There are a number of `position` values that we can assign to an element.

- `position: static` - the default
- `position: relative` - allows us to use the `top`, `bottom`, `left`, and `right` properties. It will be positioned relative to it's natural place on the page.
- `position: fixed` - it is removed from the flow of the document and placed in a fixed position within the viewport. This means it will scroll with the page, alway staying in view.
- `position: absolute` - it is removed from the flow of the document, but does no flow with the page.
- `position: sticky` - a new property that works much like fixed, but it will scroll with the page until it reaches a certain point, where it will then "stick".
