# Making A Level

## [Setting Up A Level](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987950#questions)

- Save the current scene by going to `File > Save As` and giving it a new name, `Map 1`.
- Delete the existing environment model in the scene.
- Drag and drop the`Future City/futuristic low poly city` model into the scene to add it as the new environment.
- Deactivate the `camera` attached to the `Future City` model.
- Deactivate the point lights attached to the Future City model.
- Add `mesh colliders` to each object in the `Future City` model by selecting each object (excluding the camera and point lights), going to `Add Component > Physics > Mesh Collider`.
- Adjust the scale of the `Future City` model to `0.5` on all axes by selecting the `Future City` model and changing the scale values in the Inspector.
- Change the layer of all objects in the `Future City` model to `ground`
- Save the scene and play the game to test the changes made.

## [Spawning At Different Points](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987954#questions)

- Create an empty object called `Spawn Manager`.
- Duplicate the `Player` object and remove the viewpoint and the ground check point from the duplicated player object to be used as a spawn point.
- Rename the duplicated player object to `Spawn Point`.
- Remove the `Player Controller` script and the `Character Controller` components from the `Spawn Point`.
- Place the `Spawn Points` at various locations in the game, making sure they are not inside other objects or halfway through the floor.
- Create a script called `Spawn Manager` and attach it to the `Spawn Manager` game object.
- Add all the `Spawn Points` as children to the `Spawn Manager` game object.
- Create a `public Transform[] spawnPoints` in the `SpawnManager` script to reference all the spawn points.
- In the start function of the `SpawnManager` script, deactivate all the spawn points objects so they are not visible in the game.
- Create a public function called `GetSpawnPoint` in the `SpawnManager` script that randomly picks a spawn point from the `spawn points` array.
- Create a temporary new transform called `spawnPoint` in the `PlayerController` script.
- In the `PlayerController` script, use the `GetSpawnPoint()` method from the `SpawnManager` to get a random spawn point.
- Set the position and rotation of the player to the transform of the random spawn point.

`PlayerController.cs`

```cs
void Start()
{
  //..

  // Randomly spawn player
  Transform spawnPoint = SpawnManager.Instance.GetSpawnPoint();
  transform.position = spawnPoint.position;
  transform.rotation = spawnPoint.rotation;
}
```

`SpawnManager.cs`

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class SpawnManager : MonoBehaviour
{
  public static SpawnManager Instance;
  public Transform[] spawnPoints;

  private void Awake()
  {
    if (Instance != null)
    {
      Destroy(gameObject);
      return;
    }
    Instance = this;
    DontDestroyOnLoad(gameObject);
  }

  // Start is called before the first frame update
  void Start()
  {
    foreach(Transform spawnPoint in spawnPoints)
    {
        spawnPoint.gameObject.SetActive(false);
    }
  }

  public Transform GetSpawnPoint()
  {
    return spawnPoints[Random.Range(0, spawnPoints.Length)];
  }
}
```
