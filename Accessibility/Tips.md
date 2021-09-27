- Dont use `outline: 0;` in css
- `a` tag must have `href` for it to be focusable by the keyboard/screen reader
    - better is to use </ button> for interactive item instead of href-less <a /> tags
- tabindex <= 0 is recommended best practice.

---
 

### Web Content A11y Guidelines on color contrast

A vs AA vs AAA levels are thresholds that we can aim for in our web content.

A is the minimum 
AA is acceptable 
AAA is gold standard

--- 

CSS relative units like `rem` and `em` help when browser screen is zoomed-in and zoomed-out as it allows the text to resize, as opposed to using `px`

Browsers are able to zoom in 500%, but WCAG suggests handling till 200%.



--- 

## Semantic HTML

Reference: https://webaim.org/techniques/semanticstructure/

- Use Headings and landmarks
- Start with native UI controls 
- Build semantics into templates
- Verify Assist. Tech output

Semantic HTML communicates what's on a page to users of assistive technology, reader modes, conversational UIs, search engines, and more

---


