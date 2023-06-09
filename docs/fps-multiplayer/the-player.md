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

- Open the `PlayerController` script and add the following variables:

```cs
public Transform viewPoint;
public float mouseSensitivity = 1f;
private float verticalRotStore;
private Vector2 mouseInput;
```

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

## [Making the Player Move](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987878#overview)

- Add the following to the `PlayerController` script:

```cs
public float moveSpeed = 5f;
private Vector3 moveDir, movement;

void Update()
{
  //..

  // Get the horizontal and vertical input axes and create a new direction vector from them.
  moveDir = new Vector3(Input.GetAxisRaw("Horizontal"), 0f, Input.GetAxisRaw("Vertical"));

  // Create a new movement vector from the direction vector and the object's forward and right vectors.
  // Normalize the result to ensure that the movement vector has a magnitude of 1.
  movement = ((transform.forward * moveDir.z) + (transform.right * moveDir.x)).normalized;

  // Move the object in the direction of the movement vector, scaled by the movement speed and the delta time.
  transform.position += movement * moveSpeed * Time.deltaTime;
}
```

## [Interacting With The World](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987884#overview)

- Remove the `Capsule Collider` component on the `player` and add a `Character Controller` instead
- Create a reference to the `CharacterController` in the `PlayerController` script:

```cs
public CharacterController characterController;
```

- Change the following code in the `Update()` method:

```cs
- transform.position += movement * moveSpeed * Time.deltaTime;
+ characterController.Move(movement * moveSpeed * Time.deltaTime);
```

- Add a `cube` and rotate it to create a slope. May need to change the `slope limit` on the `CharacterController` if you find you can't go up the slope!

## [Moving At Different Speeds](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987886#overview)

- To allow the player to run when `left shift` key is being held, we add the following to the `PlayerController` script:

```cs
public float runSpeed = 8f;
private float activeMoveSpeed;

void Update()
{
  activeMoveSpeed = Input.GetKey(KeyCode.LeftShift) ? runSpeed : moveSpeed;

  - movement = ((transform.forward * moveDir.z) + (transform.right * moveDir.x)).normalized;
  + movement = ((transform.forward * moveDir.z) + (transform.right * moveDir.x)).normalized * activeMoveSpeed;

  - characterController.Move(movement * moveSpeed * Time.deltaTime);
  + characterController.Move(movement * Time.deltaTime);
}
```

## [Setting Camera Position](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987890#overview)

- Remove the `camera` from the `player` so that it doesn't disappear if the player dies
- To achieve this, add the following to the `PlayerController` script:

```cs
private Camera playerCamera;

void Start() {
  // ..
  playerCamera = Camera.main;
}

// Happen after Update()
void LateUpdate() {
  playerCamera.transform.position = viewPoint.position;
  playerCamera.transform.rotation = viewPoint.rotation;
}
```

## [Adding Gravity](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987890#overview)

- Add the following to the `PlayerController` script:

```cs
void Update() {
  // ..

  // Temporary value for storing the previous movement.y value
  float yVelocity = movement.y;
  movement = ((transform.forward * moveDir.z) + (transform.right * moveDir.x)).normalized * activeMoveSpeed;
  movement.y = yVelocity;

  // Reset the yVelocity if we are grounded
  if (characterController.isGrounded) movement.y = 0f;

  // Take into account gravity
  movement.y += Physics.gravity.y * Time.deltaTime;
}
```

## [Jumping](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987894#questions)

- To enable the `player` to jump, add the following to the `PlayerController` script:

```cs
public float jumpForce = 12f;
public float gravityModifier = 2.5f;

void Update() {
  // ..

  if (Input.GetButtonDown("Jump")) {
    movement.y = jumpForce;
  }

  - movement.y += Physics.gravity.y * Time.deltaTime;
  + movement.y += Physics.gravity.y * Time.deltaTime * gravityModifier;
}
```

## [Staying Grounded](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987896#questions)

- To ensure the `player` only jumps when they are grounded, add the following to the `PlayerController` script:

```cs
public Transform groundCheckpoint;
private bool isGrounded;
public LayerMask groundLayers;
```

- Create empty object under `player` called `Ground Check Point` and set the `y` to `-0.95`

![Ground Check Point](images/ground-check-point.png)

- Attach the `Ground Check Point` to the `PlayerController` script
- Add a new layer `Ground` under `Environment` object and apply it to all the child objects as well
- Set `groundLayers` reference in the `PlayerController` script to `Ground` layer

```cs
  // Check if the player is grounded by using a Raycast to project a ray against the ground layers
  isGrounded = Physics.Raycast(groundCheckpoint.position, Vector3.down, 0.25f, groundLayers);

  // Jumping
  - if (Input.GetButtonDown("Jump")) movement.y = jumpForce;
  + if (Input.GetButtonDown("Jump") && isGrounded) movement.y = jumpForce;
```

## [Building The Game To Test](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987898#questions)

- Go to `File -> Build Settings`
- Click `Add Open Scenes` and select `Movement Testing`
- To allow it to build quickly set `Resolution` to `Windowed` and `854 x 480`
- Create a `Builds` folder and build the game
- When you play the game you will notice we can't free our mouse!

## [Freeing The Mouse](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987902#questions)

- To free the mouse, add the following at the end of the `Update()` method:

```cs
// Handle the mouse in windowed mode
if (Input.GetKeyDown(KeyCode.Escape)) Cursor.lockState = CursorLockMode.None;
else if (Cursor.lockState == CursorLockMode.None
    && Input.GetMouseButtonDown(0)) Cursor.lockState = CursorLockMode.Locked;
```

## Code Listing

#### PlayerController.cs

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public Transform viewPoint;
    public float mouseSensitivity = 1f;
    private float verticalRotStore;
    private Vector2 mouseInput;

    public bool invertLook;

    public float moveSpeed = 5f;
    public float runSpeed = 8f;
    private float activeMoveSpeed;
    private Vector3 moveDir, movement;

    public CharacterController characterController;
    private Camera playerCamera;

    public float jumpForce = 12f;
    public float gravityModifier = 2.5f;

    public Transform groundCheckpoint;
    private bool isGrounded;
    public LayerMask groundLayers;

    // Start is called before the first frame update
    void Start()
    {
        // CursorLockMode.Locked value sets the lock state of the cursor to locked,
        // which hides the cursor and locks it to the center of the game window.
        Cursor.lockState = CursorLockMode.Locked;
        playerCamera = Camera.main;
    }

    // Update is called once per frame
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

        verticalRotStore = invertLook ? verticalRotStore - mouseInput.y : verticalRotStore + mouseInput.y;
        verticalRotStore = Mathf.Clamp(verticalRotStore, -60f, 60f);

        viewPoint.rotation = Quaternion.Euler(
            verticalRotStore,
            viewPoint.rotation.eulerAngles.y,
            viewPoint.rotation.eulerAngles.z);

        moveDir = new Vector3(Input.GetAxisRaw("Horizontal"), 0f, Input.GetAxisRaw("Vertical"));
        activeMoveSpeed = Input.GetKey(KeyCode.LeftShift) ? runSpeed : moveSpeed;

        float yVelocity = movement.y;
        movement = ((transform.forward * moveDir.z) + (transform.right * moveDir.x)).normalized * activeMoveSpeed;
        movement.y = yVelocity;

        // Reset the yVelocity if we are grounded
        if (characterController.isGrounded) movement.y = 0f;

        // Check if the player is grounded by using a Raycast
        isGrounded = Physics.Raycast(groundCheckpoint.position, Vector3.down, 0.25f, groundLayers);

        // Jumping
        if (Input.GetButtonDown("Jump") && isGrounded) movement.y = jumpForce;

        // Take into account gravity
        movement.y += Physics.gravity.y * Time.deltaTime * gravityModifier;
        characterController.Move(movement * Time.deltaTime);

        // Handle the mouse in windowed mode
        if (Input.GetKeyDown(KeyCode.Escape)) Cursor.lockState = CursorLockMode.None;
        else if (Cursor.lockState == CursorLockMode.None
            && Input.GetMouseButtonDown(0)) Cursor.lockState = CursorLockMode.Locked;

    }

    void LateUpdate()
    {
        playerCamera.transform.position = viewPoint.position;
        playerCamera.transform.rotation = viewPoint.rotation;
    }
}
```
