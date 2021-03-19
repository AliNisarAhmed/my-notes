# CSS Tips 

##### Text Transform and Letter Spacing 

Whenever we use `text-transform: uppercase;` , always try to add a little letter-spacing with `letter-spacing: 1px;` so that the text is more readable.


##### rgba to lighten the font colors and blend in with background
- Use rgba (rgb with alpha) like `color: rgba(255, 255, 255, 0.65)` to soften the color and blend in with the bacground a little.

![fa30a45a2ea42a1a6aba6546b8aab868.png](fa30a45a2ea42a1a6aba6546b8aab868.png)


##### background image size and position

use `background-size: cover;` and `background-position: center;` together to center a background image across your div.


##### Background color with background Images

If we have a background Image, it's always nice to also have a background-color, so that when the image is loading, the background is of proper color, instead of just white.

***

- `tranform: scale(any);` does not affect other elements, thus does not affect layout, so other elements will see the transformed element as it was before transformation.

*** 

###### Linear Gradient in borders

`border-image: linear-gradient(to right, red, blue) 1;`

note the last 1 at the end, it is a slice

***

##### background blend mode

blend mode merges two background images, or background image with background color. `background-blend-mode: multiply;` for example keeps the darker pixels from all the merged images/backgrounds.

***

##### Flex basis with images 

Flex basis does not work properly with images specifically, need to give a min-height and min-width of 0