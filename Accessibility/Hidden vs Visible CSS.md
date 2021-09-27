
## Hidden vs Visible CSS 

1. custom `.visually-hidden` css class 

```css 
    .visually-hidden { 
        border: 0;
        clip: rect(0 0 0 0);
        height: 1px;
        width: 1px;
        margin: -1px;
        overflow: hidden;
        padding: 0;
        position: absolute;
     }
```

It is good to have the above class around in projects. 

This class makes the element "hidden visually", while it still is a part of the accessibility information (accessible to screen readers)

Useful for
    - spans that have extra text for screen-readers
    - Headings that we want to add for semantic puposes but not in visual design

2. `opacity: 0;`

Opacity = 0
    - hides the element visually
    - but keeps it in the document flow 
    - and also keeps it in a11y information

Opacity can be used to animate

3. `display: none;`

Hides it from everyone
    - visually 
    - from flow
    - from a11y info

4. `visibility: hidden;`

Element is 
    - still in document flow
    - but is visually hidden
    - and a11y information is also hidden

(kinda reciprocal of `opacity: 0`)

---

## Accessibility Tree 

A parallel structure to the DOM that uses platform/OS accessibility APIs to communicate page structure and content to assistive technologies.

Semantic HTML and CSS affects the Accessbility tree, so it is important to keep this in mind. 

---

## Focus testing Hack 

use the code below in browser console to register and event that prints out the element that is focused

```javascript
    document.body.addEventListener('focusin', (event) => {
        console.log(document.activeElement)
    })
```
