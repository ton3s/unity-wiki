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

## [Creating Impact Effect](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987912#questions)

#### Bullet Impact

- Create a `quad` object in the game world to represent the bullet impact.
- Create a empty parent object called `Bullet Impact` and add the `quad` to it
- Duplicate the `quad` object and rotate it `180` degrees so quad has two sides
- Create a material for the quad called `Bullet Impact` and apply an image from the `Art` folder called `Bullet Impact` to it to give it a more realistic appearance.
- Change the `Rendering Mode` to `Fade`
- Apply the `material` to both sides of the `quads`
- Change the scale of the `Bullet Impact` to `(0.25, 0.25, 0.25)`

#### Particle System

- Right-click on the `Bullet Impact` object in the Hierarchy window and select `Effects` -> `Particle System` from the context menu. This will add a particle system component to the bullet impact object.
- In the Particle System component, change the `Start Size` to 0.1 to create small particles.
- Change the `Duration` to 1 to control how long the particles stay visible.
- Change the `Lifetime` to 1 to control how long each particle lasts.
- Change the `Start Speed` to a low value, such as 0.1, to make the particles move slowly.
- Change the `Shape` to `Sphere` to create a circular impact area.
- Adjust the `Radius` to control the size of the impact area.
- Change the `Start Color` to `Random Between Two Colors`.
- Choose a light gray color for the first color and a dark gray color for the second color to match the bullet impact material.
- Under `Size Over Lifetime`, click on the curve axis and select `Size` to change the size of the particles over time.
- Choose the `Slower` option to create a fading effect for the particles.
- Under `Emission`, change the `Rate Over Time` to 0 to stop the particle system from continuously emitting particles.
- Under `Bursts`, click on the `+` button to add a burst of particles.
- Set the `Count` to 25 to create 25 particles in the burst.
- Turn off the `Looping` option to ensure that the particle system only emits particles once.
- Save the bullet impact object as a prefab in the `Prefabs` folder so that it can be easily reused in the game.
