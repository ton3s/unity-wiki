# Players On The Network

## [Setting Up Player For The Network](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25989188#questions)

Here are the steps to set up a player prefab to work with Photon Networking and instantiate it with multiple players:

- In Unity, open the map scene with your player GameObject.
- Create a folder named `Resources` in the `Assets` folder. This folder is `reserved` for specific purposes, like loading prefabs at runtime.
- Turn the `Player` GameObject into a `prefab` by dragging it into the `Resources` folder.
- Open the `Player` prefab and add a `Photon View` component to it. This component allows the object to be tracked by Photon and sends information to other players on the network.
- In the `Photon View` component, change the `Observed Components` setting from `Auto Find` to `Manual.` This allows you to choose the specific components you want to send over the network.
- Drag the `Player` GameObject into the `Observed Components` slot.
- In the Photon Transform View component, deselect the `Synchronize Scale` option, as the player's scale will not change during gameplay. This reduces the amount of network data being sent, improving performance.
- Save the `Player` prefab.

## [Spawning Players](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25989194#questions)

- In the `Map 1` scene, create an empty GameObject and name it `Player Spawner` .
- In your `Scripts` folder, create a new C# script called "PlayerSpawner" and attach it to the `Player Spawner` GameObject.
- Add the following code in the `PlayerSpawner` script:

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Photon.Pun;

public class PlayerSpawner : MonoBehaviour
{
  public static PlayerSpawner instance;

  private void Awake()
  {
    instance = this;
  }

  public GameObject playerPrefab;
  private GameObject player;

  // Start is called before the first frame update
  void Start()
  {
    if (PhotonNetwork.IsConnected)
    {
        SpawnPlayer();
    }
  }

  public void SpawnPlayer()
  {
    Transform spawnPoint = SpawnManager.instance.GetSpawnPoint();
    player = PhotonNetwork.Instantiate(playerPrefab.name, spawnPoint.position, spawnPoint.rotation);
  }
}
```

- In the `Launcher` script, remove the player spawning logic in the `Start()` method:

```cs
void Start()
{
  //..
  - Transform spawnPoint = SpawnManager.instance.GetSpawnPoint();
  - transform.position = spawnPoint.position;
  - transform.rotation = spawnPoint.rotation;
}
```

## [Controlling One Player](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25989196#questions/19422876)

Here is a summary of the steps to make it so players can only control themselves and not anybody else:

- Open your `PlayerControl` script.
- Add `using Photon.Pun; at the top of the script to include the Photon namespace.
- Replace `MonoBehaviour` with `MonoBehaviourPunCallbacks` to access the Photon systems.
- Modify the `LateUpdate` function to update the camera's position only if the player is in control.
- At the start of the `Update` function, add `if (photonView.IsMine)` to only execute the move input and other actions if the player is in control.
- Save the script and go back to Unity.
- Build and test the game to ensure that each player can only control their own character and not other players.

`PlayerControl.cs`

```cs
using Photon.Pun

public class PlayerController : MonoBehaviourPunCallbacks
{
  //..

  void Update()
  {
    // Check if we have authority over the current player
    if (photonView.IsMine)
    {
      //..
    }
  }

  void LateUpdate()
  {
    // Check if we have authority over the current player
    if (photonView.IsMine)
    {
      //..
    }
  }
}
```
