# Room Browsing, Nicknames & Starting a Game

## [Laying Out The Room Browser](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987986#questions)

- Duplicate the existing `Room Panel` and rename it to `Room Browser Panel`.
- Activate the `Room Browser Panel`, and update the title text to say `Find Room`.
- Create a scrollable area for the list of rooms by adding a `UI > ScrollView` component to the `Room Browser Panel`.
- Adjust the size of the `ScrollView` to your desired dimensions (e.g., `600w x 700h`).
- Locate the content area inside the ScrollView, and adjust its size to be larger than the full size of the ScrollView (e.g `1000`). This will activate the scrollbars.
- Increase the scroll sensitivity of the `ScrollView` to make it scroll faster (e.g., `100`).
- Customize the appearance of the vertical scrollbar, such as changing its color and disabling the image on the scrollbar background.
- Add a `Button` prefab to the `Content` area of the `ScrollView > Viewport`.
- Add a `Vertical Layout Group` component to the `Content` area to arrange the room buttons vertically with padding and spacing.
- Disable the `Force Expand Height` option in the `Vertical Layout Group` component.
- Add a `Content Size Fitter` component to the `Content` area and set it to `Vertical Fit: Min Size`. This will adjust the size of the `Content` area based on the number of buttons.
- Test the scrolling functionality by duplicating the room buttons and running the game. - Adjust the `ScrollView` settings as needed, such as changing the movement type to `Clamped` if you do not want the elastic bounce effect.

![Room Browser](images/room-browser.png)
