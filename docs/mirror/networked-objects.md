## Networked Objects in Mirror

Following is an example of how to spawn a network pickup using Mirror

1. Define all the networked prefabs in the Registered Spawnable Prefabs array in the Network Manager

2. In the Custom Network Manager in OnStartServer() method spawn the first pickup

```csharp
// Spawn first pickup
SpawnManager.Instance.SpawnPickups();
```

3. From the Spawn Manager singleton, call the Command method CmdSpawnPickup() which then creates the prefab and spawns it on the Network Server

```csharp
[Command(requiresAuthority = false)]
void CmdSpawnPickup(string pickupName, Vector3 spawnPos)
{
	// Debug.Log($"Spawning {pickupName}");
	GameObject pickup = Instantiate(GetSpawnablePrefab(pickupName));
	pickup.transform.position = spawnPos;
	NetworkServer.Spawn(pickup);
}
```

4. Create a separate script and attach to the networked prefab. Use this script to handle all the desired responses in a networked way.
