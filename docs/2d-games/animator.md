## Animator

- Create an `Animator Controller` call `Player_AC`
- Add `Animator Controller` to the game object where the `Sprite Renderer` is located
- Create an `Animation` and drag the desired sprites
- Select hamburger icon and select `Show Sample Rate`

### To Create More Animations

- Select `Animator`
- Select `Create New Clip`

### Transition Between Animations

- Select `Parameters` and click `+` and add desired type.
- Select `Make Transition` between animation states
- Go to `Conditions` and add `Parameters` to move between states
- Uncheck `Exit Time`
- Set `Transition Duration` to 0

### Flipping Character

- To flip character, rotate on the `Y` axis 180 degrees

```c#
private void Flip()
{
	facingDir *= -1;
	facingRight = !facingRight;
	transform.Rotate(0, 180, 0);
}

private void FlipController()
{
	if (rb.velocity.x > 0 && !facingRight) Flip();
	else if (rb.velocity.x < 0 && facingRight) Flip();
}
```
