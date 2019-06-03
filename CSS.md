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
