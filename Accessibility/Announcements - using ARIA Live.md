
## Announcements - using ARIA Live Regions

These elements, when some text is appended to them, the Screen Reader will announce them. They are usually visually hidden.

Use Cases: 
    - Asynchronous save / update etc. 
    - Combobox usage / list filtering 
    - Chat widgets
    - Title Changes

ARIA Live Region Roles: https://www.w3.org/TR/wai-aria/#live_region_roles

```html
    <div role="status"></div>
    <div role="alert"></div>
    <div aria-live="polite"></div>
    <div aria-live="assertive"></div>
```

Politeness level depends on the use case

Example implementation: 

```javascript
const LiveRegion = () => {
    
    const inputRef = useRef(null);
    const [message, setMessage] = useState('Nothing here.');
    
    const submitHandler = (event) => {
        event.preventDefault();
        
        setMessage(inputRef.current.value);
    }
    
    return (
        <div>
            <div role="status">
                <p>{message}</p>
            </div>
            <form onSubmit={submitHandler}>
                <label>
                    Enter text for <br />
                    <input type="text" ref={inputRef}/>
                </label>
                <button type="submit">Start</button>
            </form>
        </div>
    )
}
```