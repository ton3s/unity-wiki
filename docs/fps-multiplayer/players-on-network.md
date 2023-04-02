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
- In your `Scripts` folder, create a new C# script called `PlayerSpawner` and attach it to the `Player Spawner` GameObject.
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

## [Auto Returning To Menu](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25989198#questions)

- Create a new script called `MatchManager` in the Scripts folder.
- In the scene, create an empty GameObject named `MatchManager` and attach the MatchManager script to it.
- Open the MatchManager script and add the following:

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Photon.Pun;
using UnityEngine.SceneManagement;

public class MatchManager : MonoBehaviour
{
  public static MatchManager instance;

  private void Awake()
  {
    instance = this;
  }

  // Start is called before the first frame update
  void Start()
  {
      // If we are not connected to the network, load the main menu
    if (!PhotonNetwork.IsConnected)
    {
      SceneManager.LoadScene(0);
    }
  }
}
```

- Save the script and test it in Unity to ensure that the main menu loads when not connected to the network.
- To fix the lighting issue in the editor, stop the game, then open `Window > Rendering > Lighting`.
- Dock the Lighting window next to the Inspector.
- Create new lighting settings for general use in your game by clicking `New Lighting Settings` and saving it in the `Assets` folder.
- With the scene open, scroll down in the Lighting window and click `Generate Lighting` to examine and apply the lighting settings to the scene.
- Save the scene and test the game again to see the improved lighting in the editor.

## [Showing Player Impacts](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25989200#questions)

- Create a `Player Hit Impact` effect prefab by duplicating an existing `Bullet Impact` prefab and modifying it to suit the game's theme.
- Add the new prefab to the `Resources` folder.
- Add a `DestroyOverTime` script to the prefab so it destroys itself after a specified time.
- Add a `Photon View` component to the prefab and set it up to synchronize the position.
- In the `PlayerController` script, add a `public GameObject playerHitImpact` prefab.
- Assign the `Player tag` to the player object in Unity.
- Modify the shoot function in the Player Controller script to check if the object hit has the Player tag. If it does, instantiate the player hit impact effect at the hit point using Photon Network. If not, instantiate the normal bullet impact effect.
- Add a debug log to display the name of the player being hit (this is for testing purposes only and can be removed later).

`PlayerController.cs`

```cs
public GameObject playerHitImpact;

private void Shoot() {
  //..

  if (Physics.Raycast(ray, out RaycastHit hit))
  {
    // ..

    if (hit.collider.gameObject.CompareTag("Player"))
    {
      Debug.Log("We hit " + hit.collider.gameObject.GetPhotonView().Owner.NickName + "!");
      PhotonNetwork.Instantiate(playerHitImpact.name, hit.point, Quaternion.identity);
    }
    else
    {
      GameObject bulletImpactObject = Instantiate(bulletImpact, hit.point + (hit.normal * 0.02f), Quaternion.LookRotation(hit.normal, Vector3.up));
      Destroy(bulletImpactObject, 5f);
    }
  }
}
```

`DestroyOverTime.cs`

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyOverTime : MonoBehaviour
{
  public float lifetime = 1.5f;

  // Start is called before the first frame update
  void Start()
  {
    Destroy(gameObject, lifetime);
  }
}
```

## [Shooting With RPCs](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25989204#questions)

In order to send information across the network to let others know when a player has been hit:

- In the `PlayerController` script, create a new public function called `DealDamage` below the `Shoot` function.
- Before the `DealDamage` function, type in square brackets and `PunRPC` to make it a Photon Remote Procedure Call (RPC). This will allow the function to run at the same time on every copy of the player on the network.
- Inside the `DealDamage` function, add a debug log that says `I've been hit` for now.
- Modify the `DealDamage` function to take in a string parameter called `Damager` and update the debug log to include the damager's name.
- When shooting, get the PhotonView component of the object that was hit.
- Call the `DealDamage` RPC function using the PhotonView.RPC method, passing in the function's name as a string, the target (in this case, `RpcTarget.All`), and the shooter's nickname as the `Damager` parameter.
- Save the script, build and run the game to test the functionality.

Now, when shooting a player, the `I've been hit` message will be displayed along with the name of the player who inflicted the damage. This basic shooting functionality will serve as a foundation for further development, such as updating player health and handling player death.

```cs
[PunRPC]
public void DealDamage(string damager)
{
  // This will be sent to every player
  Debug.Log("I've been hit by " + damager);
}

private void Shoot()
{
  //..

  if (Physics.Raycast(ray, out RaycastHit hit))
  {
    // ..

    if (hit.collider.gameObject.CompareTag("Player"))
    {
      // ..
      hit.collider.gameObject.GetPhotonView().RPC("DealDamage", RpcTarget.All, photonView.Owner.NickName); ;
    }
  }
}
```
