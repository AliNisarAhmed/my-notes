## Progressive Enhancement

Emphasize core web page content first, then add layers of presentation and features on top asn browsers/network connections allow.

* Turn off JavaScript - content should be readable without JS enabled
* Provide accessible baseline markup
* Add ARIA with scripting
* Prioritize core user flows

Example Tablist - which will work even when JS is disabled: 

```javascript
import React, { useEffect, useState } from 'react'

const TabList = ({ items = [] }) => {
  const [isClient, setClient] = useState(false)
  
  // This effect will only run when JS is enabled, hence the default state for isClient = false will be there if JS is disabled.
  useEffect(() => {
    setClient(true)
  }, [])
    
  return (
      // we want to apply role=tablist only when JS is enabled
      // if JS is off, the list should just be a regular list as it does not have any interactivity.
    <ul {...isClient && { 'role': 'tablist' }}>
      {items.map(item => (
        <li key={item.id}
          {...isClient && { 'role': `tab` }}
        >
          {item.label}
        </li>
      ))}
    </ul>
  )
}

export default TabList
```