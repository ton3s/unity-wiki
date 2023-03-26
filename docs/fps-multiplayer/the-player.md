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

## [Making The Camera Move](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987872#overview)

- Open the `PlayerController` script and add a `public Transform viewPoint` and add the reference to the `View Point` in the `inspector`
- Add a `public float mouseSensitivity = 1f`
- Add a `private float verticalRotStore` to store the vertical rotation for looking up and down
- Add a `private Vector2 mouseInput`
- In the `Update()` method add the following code to allow mouse movement along the x axis to rotate the players view along the y axis:

```cs
void Update()
{
    // Get the mouse input on the x and y axes and multiply it by a sensitivity value
    mouseInput = new Vector2(
        Input.GetAxisRaw("Mouse X"),
        Input.GetAxisRaw("Mouse Y")) * mouseSensitivity;

    // Set the rotation of the object to a new quaternion that has the same x and z angles as before,
    // but has the y angle increased by the mouse input on the x axis.
    transform.rotation = Quaternion.Euler(
        transform.rotation.eulerAngles.x,
        transform.rotation.eulerAngles.y + mouseInput.x,
        transform.rotation.eulerAngles.z);
}
```

## [Looking Up & Down](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987874#overview)

- Add the following code in `Update()` to allow the player to look up and down while clamping the viewing angle to 60 degrees:

```cs
void Update()
{
    // ...

    // Accumulate the vertical rotation based on the user's mouse input.
    verticalRotStore += mouseInput.y;

    // Clamp the vertical rotation between -60 and 60 degrees to prevent the camera from rotating too far up or down.
    verticalRotStore = Mathf.Clamp(verticalRotStore, -60f, 60f);

    // Set the rotation of the camera to a new quaternion that has the x angle set to the vertical rotation value, which rotates the camera up and down.
    // The y and z angles are set to the current y and z angles of the camera to preserve the horizontal orientation.
    viewPoint.rotation = Quaternion.Euler(
        verticalRotStore,
        viewPoint.rotation.eulerAngles.y,
        viewPoint.rotation.eulerAngles.z);
}
```

## [Inverting & Hiding The Mouse](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987876#overview)

- Add a `public bool invertLook` variable
- Change `verticalRotStore` based on this boolean value

```cs
- verticalRotStore += mouseInput.y;
+ verticalRotStore = invertLook ? verticalRotStore - mouseInput.y : verticalRotStore + mouseInput.y;
```

- Hide the mouse by updating the `Start()` method with the following:

```cs
void Start()
{
  // CursorLockMode.Locked value sets the lock state of the cursor to locked,
  // which hides the cursor and locks it to the center of the game window.
  Cursor.lockState = CursorLockMode.Locked;
}
```
