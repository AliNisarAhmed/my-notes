
## Accessible Dropdown example

Below is React code: 

From: https://github.com/marcysutton/js-a11y-workshop/blob/solutions/src/components/better/dropdown.js

Codepen: https://codepen.io/marcysutton/pen/JgjYVv

```typescript

const Dropdown = ({ activatorText = 'Dropdown', items = [] }) => {
    const activatorRef = useRef(null);
    const dropdownListRef = useRef(null);
    const [ isOpen, setIsOpen ] = useState(false);
    
    // This effect is being used to focus on the first item in the list when the dropdown is toggled ON, and set up a mousedown event handler to close the dropdown when mouseclick is outside the dropdown
    useEffect(() => {
        if (isOpen) {
            // when isOpen is changed to true, focus on the first element
            dropdownListRef.current.querySelector('a').focus();
            
            document.addEventListener('mousedown', clickOutsideHandler);
        } else {
            document.removeEventListener('mousedown', clickOutsideHandler);
        }
        
        // clean up on unmount
        return function cleanup () { 
            document.removeEventListener("mouseup", clickOutsideHandler) |
        }
    }, [isOpen])
    
    const clickOutsideHandler = (event) => {
        if (dropdownListRef.current.contains(event.target) || activatorRef.current.contains(event.target)) {
            return;
        }
        
        setIsOpen(false);
    }
    
    const clickHandler = (event) => {
        setIsOpen(prev => !prev);
    }
    
    const keyHandler = (event) => {
        // when the user presses Esc, close the dropdown if open
        if (event.code === 27 && isOpen) {
            setIsOpen(false);
        }
    }
    
    return (
        <div
            className="dropdown-wrap"
            onKeyUp={keyHandler}
        >
            <button
                aria-haspopup="true"
                aria-controls="dropdown1"
                onClick={clickHandler}
                ref={activatorRef}
                className="dropdown-activator"
            >
                {activatorText}
            </button>
           <ul
                id="dropdown1"
                ref={dropdownListRef}
                role="list"
                // tabIndex="-1" // makes the list itself focusable, but focus is better sent to the first list item
                className={`dropdown-itemlist ${isOpen ? 'active' : ''}`}
            >
             {items.map((item, index) => {
               return (
                   <li key={index} role="listitem">
                       <a href={item.url}>{item.text}</a>
                   </li>
                )
             })}
           </ul>
        </div>
    )
}
```

### aria-popup 

`aria-popup`: Indicates the availability and type of interactive popup element, such as menu or dialog, that can be triggered by an element.

A popup element usually appears as a block of content that is on top of other content. Authors MUST ensure that the role of the element that serves as the container for the popup content is menu, listbox, tree, grid, or dialog, and that the value of aria-haspopup matches the role of the popup container.

More: https://developer.mozilla.org/en-US/docs/Web/API/Element/ariaHasPopup

#### aria-controls 

Identifies the element (or elements) whose contents or presence are controlled by the current element.

`aria-controls`: The `aria-controls` attribute creates a cause and effect relationship. It identifies the element(s) that are controlled by the current element, when that relationship isn’t represented in the DOM. For example a `button` that controls the display of information contained within a `div`



```javascript
<button onclick="showInfo();" aria-controls="I">Show info</button>
…
<div id="I">Information.</div>
```

The more widely the two elements are separated in the DOM, the more useful the `aria-controls` attribute becomes.

---