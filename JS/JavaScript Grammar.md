# JavaScript Grammar

---

#### DOM vs Media
`document.addEventListener('DOMContentLoaded', function () {})`, the function is used to fire events after all DOM elements have been loaded, but the media may not have loaded.

To make sure media has loaded, use `window.onload = function () {...};`
