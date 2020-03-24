# Units

- Absolute
  - px, pt, mm etc.
- Percentage
  - Relative to their parent
- Relative
  - Relative to Font-size - `em` and `rem`
  - Relative to view-port - `vw` `vmin` `vh` `vmax`

* `px` used to be a fixed unit (a got on the screen)

  - but now it follows the **reference pixel**, it is now about 1/96th of an inch, so that it is uniform across all devices of varying quality.

* `%` units are always a percentage of their parent width.

* `em` are relative to the element's font-size

  - Font-size is an inherited property, so if you don't declare it anywhere, it's getting it from the body (or the default which is normally 16px)
  - The problem with `em` is that, since it derives from parent's sizes, the sizes accumulate/compound. 20px > 1.5 em > 1.5 em will result in 20px > 30px > 45px
  - Don't use `em`

- `rem` - root em

Rule of thumbs on when to use what

- Font-size = `rem`
- Padding and margin - `em`
  - On padding, margin (and width), `em` is relative to the element's font size **!!!!**
  - This makes `em` useful for relatively styled spacing, i.e. styling spaces relative to the font-sizes of the elements (for example, perfectly scaling buttons in terms of space around the text inside of the button, when font-sizes increase or decrease)
- widths - `em` or `%`
