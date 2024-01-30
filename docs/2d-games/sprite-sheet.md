## Sprite Sheet

### Slice the Sheet

- Set `Sprite Mode` to `Multiple`
- Go to `Sprite Editor` and select `Slice`
- Set `Type` of `Grid By Cell Size`
- X: 69, y: 44
- Apply

### Size

- Change `Pixel Per Unit` to 16

### Image Quality

- Set `Compression` to `None`
- Set `Filter Mode` to `Point (no filter)`
- Apply

### Center Sprite

- Remove the `Sprite Renderer` component from the `Player`
- Create a empty child object called `Animator`
- Set the `x` position to 0.4

### Final Settings

- Add `Capsule Collider 2D` to the `Player`
- Set `Freeze Rotation` on `Z`
- Set `Collision Detection` to `Continuous`
- Set `Interpolate` to `Interpolate`

The Interpolate setting on a Rigidbody provides two options to smooth the appearance of a Rigidbody’s motion if it appears jittery at run time. These options are Interpolate and Extrapolate.

Both interpolation and extrapolation calculate the pose of the Rigidbody (that is, the position and rotation) between physics updates. Which one you should choose depends on which option produces the best visual outcome for your use case.
