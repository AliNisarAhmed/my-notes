# Clickjacking

A UI "redress" attack.

Can be used to capture keystrokes as well.


## Defense 

- `X-Frame-Options` an HTTP Response header 
- This header can be used to indicate whether or not a browser should be allowed to render a page in a `frame` , `iframe` , `embed` or `object`. 
- Sites can use this to avoid click-jacking attacks, by ensuring that their content is not embedded into other sites. 
    - values are 
        - `DENY`
        - `SAMEORIGIN`
        - `ALLOW-FROM https://example.com/`

- Chrome/Safari don't respect allow-from. Use `frame-ancestors` CSP directive instead.
- This applies to the top level frame.