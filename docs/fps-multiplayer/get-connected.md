# Get Connected

## [Setting Up Photon](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987962#questions)

- Install PUN2 from the Unity Asset Store: https://assetstore.unity.com/packages/tools/network/pun-2-free-119922
- Before importing into our project, de-select the demo checkboxes
- Create an account here: https://www.photonengine.com/
- Create a new app and note the `app id`. This will be required as part of the PUN setup in Unity.
- Under `Assets/Photon/PhotonUnityNetworking/Resources/PhotonServerSettings`, change `Fixed Region` to `us`

## [Laying Out The Connection Menu](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987964#questions)

- Create a new scene for the main menu and save it in the `scenes` folder as `main menu`.
- Set up the basic buttons for the menu by creating a UI canvas, renaming it to `menu canvas`, and adjusting the canvas settings to scale with screen size.
- Add a title text to the top of the canvas to display the name of the game.
- Create buttons for the menu system by adding UI button elements and customizing their appearance, size, and position.
- Convert the button into a prefab by dragging it into the prefabs folder. This allows for easy reuse of the button design in different scenes.
- Create additional buttons for different menu options such as `Find Room`, `Create Room`, and `Quit Game`. Customize the text and appearance of each button as needed.
- Organize the buttons using a Vertical Layout Group component in a parent object called `menu buttons`. Adjust the positioning and spacing of the buttons within the layout.
- Add a loading panel by creating a UI panel element. Customize the panel's appearance, size, and position to display loading information.
- Add loading text to the loading panel using a UI TextMeshPro element. Customize the text appearance and positioning.
- Import a 3D model of a city as a background for the menu scene. Adjust the position and rotation of the camera to create a visually appealing view.
- Save the scene and proceed to the next steps, which will involve connecting the menu buttons to the actual game functionality.
