# The Player

## [Setting Up](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987866#overview)

- Import the assets using `The Unarchiver` app on the Mac
- Create a `plane` with the scale of `100, 100, 100`
- Create a dark grey material for the `ground` and add to the `plane`
- Create a `capsule` to represent the `player` with an orange material
- Create a `cube` called `object` with a blue material
- Duplicate, scale and rotate the `cubes` in pairs to create buildings
- Create a `cube` as VR googles for the `player` so we know what way they are facing
- Make the `cube` a child of the `player` and set the default of the googles to the z-axis (default)
- Add both the `plane` and the `cubes` as a child of to a `environment` object
- Save the `scene`

## [Understanding Looking Around](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987868#overview)

- Make the `camera` a child of the `player` so it moves with the player
- Position the `camera` at eye level with the `player`
- Create an empty `view point` object and move the `camera` as a child. This will allow the player to look up and down without the body also rotating!
- Rotate the `player` and observe the `game camera` rotating as well. The `player` body will be used to look left to right and the `view point` will be used to look up and down

> When rotating around the `view point` you may need to adjust the `Clipping Planes` of the camera if you are seeing the inside of the goggles

- Create a `PlayerController` script and attach to the `player`
