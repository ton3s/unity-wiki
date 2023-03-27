# Shooting & Weapons

## [Shooting](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987910#questions)

- In order for the `player` to shoot, add the following in the `PlayerController` script:

```cs
void Update() {
  // ..

  // Shoot if player left clicks
  if (Input.GetMouseButtonDown(0)) {
    Shoot();
  }

  // Handle the mouse in windowed mode
  // ..
}

private void Shoot() {
    // Create a new ray that goes from the center of the screen outwards.
    Ray ray = playerCamera.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0f));

    // Set the origin of the ray to the position of the player's camera.
    ray.origin = playerCamera.transform.position;

    // If the raycast hits an object in the scene...
    // out passes by reference allowing 'hit' to be populated with return values
    if (Physics.Raycast(ray, out RaycastHit hit))
    {
        // ...output the name of the object to the console.
        Debug.Log("We hit " + hit.collider.gameObject.name);
    }
}
```

- Add a `cube` as a child of the `View Point` to represent the `Gun Placeholder`
- Add a `material` to the `Gun Placeholder`
- Remove the `Box Colliders` from both the `Goggles` and the `Gun Placeholder`

![Gun Arm](images/gun-arm.png)
