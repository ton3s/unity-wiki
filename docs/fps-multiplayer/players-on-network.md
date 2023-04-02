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
