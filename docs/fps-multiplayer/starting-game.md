# Room Browsing, Nicknames & Starting a Game

## [Laying Out The Room Browser](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987986#questions)

- Duplicate the existing `Room Panel` and rename it to `Room Browser Panel`.
- Activate the `Room Browser Panel`, and update the title text to say `Find Room`.
- Create a scrollable area for the list of rooms by adding a `UI > ScrollView` component to the `Room Browser Panel`.
- Adjust the size of the `ScrollView` to your desired dimensions (e.g., `600w x 700h`).
- Locate the content area inside the ScrollView, and adjust its size to be larger than the full size of the ScrollView (e.g `1000`). This will activate the scrollbars.
- Increase the scroll sensitivity of the `ScrollView` to make it scroll faster (e.g., `100`).
- Customize the appearance of the vertical scrollbar, such as changing its color and disabling the image on the scrollbar background.
- Add a `Button` prefab to the `Content` area of the `ScrollView > Viewport`.
- Add a `Vertical Layout Group` component to the `Content` area to arrange the room buttons vertically with padding and spacing.
- Disable the `Force Expand Height` option in the `Vertical Layout Group` component.
- Add a `Content Size Fitter` component to the `Content` area and set it to `Vertical Fit: Min Size`. This will adjust the size of the `Content` area based on the number of buttons.
- Test the scrolling functionality by duplicating the room buttons and running the game. - Adjust the `ScrollView` settings as needed, such as changing the movement type to `Clamped` if you do not want the elastic bounce effect.

![Room Browser](images/room-browser.png)

## [Making The Room Browser Work](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987988#questions)

- Create a new script named `RoomButton` in the scripts folder.
- Attach the `RoomButton` script to the room button GameObject in your game.
- Open the RoomButton script in a code editor and add the following code:

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using TMPro;
using Photon.Realtime;

public class RoomButton : MonoBehaviour
{
  public TextMeshProUGUI buttonText;

  private RoomInfo info;

  public void SetButtonDetails(RoomInfo inputInfo)
  {
    info = inputInfo;
    buttonText.text = info.Name;
  }
}
```

- Add the following code in the `Launcher` script to manage the rooms in the game:

```cs
public GameObject roomBrowserScreen;
public RoomButton theRoomButton;
private List<RoomButton> allRoomButtons = new List<RoomButton>();
private Dictionary<string, RoomInfo> cachedRoomsList = new Dictionary<string, RoomInfo>();

public void OpenRoomBrowser()
{
  CloseMenus();
  roomBrowserScreen.SetActive(true);
}

public void CloseRoomBrowser()
{
  CloseMenus();
  menuButtons.SetActive(true);
}

// This method is called when the room list is updated.
public override void OnRoomListUpdate(List<RoomInfo> roomList)
{
  // Update the cached room list with the new room list information.
  UpdateCachedRoomList(roomList);
}

// This method updates the cached room list based on the provided room list.
public void UpdateCachedRoomList(List<RoomInfo> roomList)
{
  // Iterate through the room list.
  for (int i = 0; i < roomList.Count; i++)
  {
    RoomInfo info = roomList[i];

    // If the room has been removed from the list, remove it from the cached room list.
    if (info.RemovedFromList)
    {
      cachedRoomsList.Remove(info.Name);
    }
    else
    {
      // Otherwise, update or add the room to the cached room list.
      cachedRoomsList[info.Name] = info;
    }
  }

  // Update the room list buttons with the updated cached room list.
  RoomListButtonUpdate(cachedRoomsList);
}

// This method updates the room list buttons based on the provided cached room list.
void RoomListButtonUpdate(Dictionary<string, RoomInfo> cachedRoomList)
{
  // Clean up existing room buttons.
  foreach (RoomButton rb in allRoomButtons)
  {
    Destroy(rb.gameObject);
  }
  allRoomButtons.Clear();
  theRoomButton.gameObject.SetActive(false);

  // Iterate through the cached room list and create new room buttons.
  foreach (KeyValuePair<string, RoomInfo> roomInfo in cachedRoomList)
  {
    // Instantiate a new room button and set its parent.
    RoomButton newButton = Instantiate(theRoomButton, theRoomButton.transform.parent);

    // Set the new button's details with the room information.
    newButton.SetButtonDetails(roomInfo.Value);

    // Activate the new button.
    newButton.gameObject.SetActive(true);

    // Add the new button to the list of all room buttons.
    allRoomButtons.Add(newButton);
  }
}
```

- In Unity, assign the `theRoomButton` and `roomBrowserScreen` GameObjects to their respective fields in the `Launcher` script component.
- Create functions to open and close the room browser in the Launcher script, then wire up the `Find Room` button to call the `OpenRoomBrowser()` function and the `Leave` button to call the `CloseRoomBrowser()` function.
- Test the game in Unity to ensure the room buttons are created, updated, and destroyed correctly when browsing available rooms.

![Find Room](images/find-room.png)

## [Joining A Room](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987990#questions)

- In the `Launcher` script, create a new function called `JoinRoom()` that takes in a `RoomInfo` object as an argument called inputInfo. This function will handle the process of joining a room.
  In the `RoomButton` script, create another function called `OpenRoom()`. This function will tell the launcher script's instance to run the `JoinRoom()` function, passing in the stored information about the room that the user wants to join.
- Back in the `Launcher` script, within the `JoinRoom()` function, use the `PhotonNetwork.JoinRoom` method to join the room by passing in the room name from the `inputInfo` object: `inputInfo.Name`.
- After joining the room, close the open menus, update the loading text to `Joining Room`, and activate the loading screen.
- Now, go back to Unity, and on the `Room Browser Panel`, find the `Room Button` object under the `Content` object.
- Configure the `Room Button` object to run the `RoomButton.OpenRoom` function when clicked.
- Test the implementation by running two instances of the game, one within Unity and another using an older build of the game.
- Create a room in one instance and use the `Room Browser` in the other instance to find and join the room.
- Test the functionality of leaving and rejoining the room.

`Launcher.cs`

```cs
public void JoinRoom(RoomInfo inputInfo)
{
  PhotonNetwork.JoinRoom(inputInfo.Name);
  CloseMenus();
  loadingText.text = "Joining Room";
  loadingScreen.SetActive(true);
}
```

`RoomButton.cs`

```cs
 public void OpenRoom()
{
  Launcher.instance.JoinRoom(info);
}
```

## [Quitting The Game](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987992#questions)

- Open the `Launcher` script in your code editor.
- At the bottom of the `Launcher` script, add the following:

```cs
public void QuitGame()
{
  #if UNITY_EDITOR
  UnityEditor.EditorApplication.isPlaying = false;
  #endif
  Application.Quit();
}
```

## [Listing Players](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987994#questions)

- In Unity, make necessary changes to the room panel by duplicating the room browser panel and modifying it accordingly.
- Create a UI TextMeshPro object in the scroll view's content and name it `Player Name Label`. Make sure it's centered and deactivated by default.
- In the `Launcher` script, create references to the `playerNameLabel` object, a list of `TextMeshProUGUI` objects called `allPlayerNames`.
- Assign a random nickname to each player as they join the lobby by setting `PhotonNetwork.NickName` to a random number in string format.
- Create a new private function called `ListAllPlayers()` in the launcher script.
- Inside the `ListAllPlayers()` function, destroy any existing player name labels in the `allPlayerNames` list, then clear the list.
- Get the list of players in the room from `PhotonNetwork` and store it in a `Player` array called `players`.
- Loop through the `players` array and instantiate a `newPlayerLabel` for each player. - Set the text of the label to the player's nickname, activate the label, and add it to the `allPlayerNames` list.
- Call the `ListAllPlayers` function when a player joins a room.
- Assign the `playerNameLabel` object in Unity to the appropriate field in the `Launcher` script.
- Test the implementation by building and running the game. Check if the list of players updates correctly when a new player joins the room.
- To fix the issue of not seeing all the players when a new player joins, you need to update the list of players whenever a new player joins the room.

```cs
public TextMeshProUGUI playerNameLabel;
private List<TextMeshProUGUI> allPlayerNames = new List<TextMeshProUGUI>();

public override void OnJoinedLobby()
{
    //..

    // Generate a random nickname for a player
    PhotonNetwork.NickName = Random.Range(0, 1000).ToString();
}

public override void OnJoinedRoom()
{
  // ..
  ListAllPlayers();
}

private void ListAllPlayers()
{
  // Clear out existing players
  foreach(TextMeshProUGUI player in allPlayerNames)
  {
    Destroy(player.gameObject);
  }
  allPlayerNames.Clear();

  // Retrieve the current list of players and save to the array
  Player[] players = PhotonNetwork.PlayerList;
  for (int i = 0; i < players.Length; i++)
  {
    TextMeshProUGUI newPlayerLabel = Instantiate(playerNameLabel, playerNameLabel.transform.parent);
    newPlayerLabel.text = players[i].NickName;
    newPlayerLabel.gameObject.SetActive(true);
    allPlayerNames.Add(newPlayerLabel);
  }
}
```

![Random Nicknames](images/random-nicknames.png)

## [Updating The Player List](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987996#questions)

- In your `Launcher` script, add two new override functions:
  - `public override void OnPlayerEnteredRoom(Player newPlayer)`: This function will be executed when a player enters the room.
  - `public override void OnPlayerLeftRoom(Player otherPlayer)`: This function will be executed when a player leaves the room.
- In the `OnPlayerEnteredRoom()` and `OnPlayerLeftRoom()` function, relist all the players in the room by clearing the list and adding the players again.

> **Note**: Make sure to update the `Leave Room` button in the room panel to actually call the `Leave Room` function instead of just closing the room browser. This will ensure that the player properly leaves the room and avoids any errors when attempting to create a new room while still being in another room.

```cs
public override void OnPlayerEnteredRoom(Player newPlayer)
{
  ListAllPlayers();
}

public override void OnPlayerLeftRoom(Player otherPlayer)
{
  ListAllPlayers();
}
```

## [Players Nickname](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25987998#questions/15685310)

- Duplicate the `Create Room` panel and rename it as `Name Input Panel`.
- Modify the input field placeholder to say `Enter Username Here`.
- Update the launcher script with new references:

```cs
public GameObject nameInputScreen;
public TMP_InputField nameInputField;
private bool hasSetNickname;
```

- In the `OnJoinLobby()` function, add the following code:

```cs
if (!hasSetNickname) {
  CloseMenus();
  nameInputScreen.SetActive(true);
}
```

- Update the `CloseMenus()` function to set `nameInputScreen.SetActive(false)`:

```cs
void CloseMenus()
{
  //..
  nameInputScreen.SetActive(false);
}
```

- Create a new function `SetNickname()`:

```cs
public void SetNickname()
{
  if (!string.IsNullOrEmpty(nameInputField.text))
  {
      PhotonNetwork.NickName = nameInputField.text;
      CloseMenus();
      menuButtons.SetActive(true);
      hasSetNickname = true;
  }
}
```

- In Unity, assign the `nameIputField` and `nameInputSCreen` to the appropriate references in the `Launcher` script.
- Update the button in `Name Input Panel` to call `SetNickname()` function.
- To store the player name between game sessions, use `PlayerPrefs`:

```cs
public override void OnJoinedLobby()
{
  //..
  if (!hasSetNickname)
  {
    //..
    if (PlayerPrefs.HasKey("playerName"))
    {
      nameInputField.text = PlayerPrefs.GetString("playerName");
    }
  }
  else
  {
    PhotonNetwork.NickName = PlayerPrefs.GetString("playerName");
  }
}

public void SetNickname()
{
  if (!string.IsNullOrEmpty(nameInputField.text))
  {
    PhotonNetwork.NickName = nameInputField.text;
    PlayerPrefs.SetString("playerName", nameInputField.text);
    //..
  }
}

```

- Test the game in Unity to see if the player names are being saved and displayed correctly in the game.

![Set Username](images/set-username.png)

## [Starting The Game](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25988002#questions)

- Duplicate the `Leave` button in the room panel and name it `Start Game`.
- Ensure the button has `no function` assigned to avoid any accidental actions.
- Go to the `Launcher` script in the menu canvas.
- Set `PhotonNetwork.AutomaticallySyncScene = true` to allow the Photon network to sync the scene information.
- Create a public string variable called `levelToPlay` and assign the level name (`Scene Name`) you want to load.
- Create a public function named `StartGame()` and add the following code:

```cs
public void StartGame()
{
  PhotonNetwork.LoadLevel(levelToPlay);
}
```

- Build and test the game to make sure the button works correctly.
- Create a `public GameObject startButton` in the launcher script.
- Check if the player is the master client using `PhotonNetwork.IsMasterClient`.
- If the player is the master client, set the start button to active, otherwise set it to inactive.

```cs
public override void OnJoinedRoom()
{
  //..
  CheckTheMaster();
}

private void CheckTheMaster()
{
  // Check if we are the master
  if (PhotonNetwork.IsMasterClient)
  {
    startButton.SetActive(true);
  }
  else
  {
    startButton.SetActive(false);
  }
}
```

- Assign the start button in the menu canvas to the appropriate button in the room panel.
- Save and build the game again to test the changes.
- After completing these steps, the start game button should only be active for the master client, and pressing it should load the specified level for all players in the room.

## [Switching Masters](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25988004#questions)

- To ensure the `Start Game` button is available for a new master if the current master leaves the room, add the following:

```cs
public override void OnMasterClientSwitched(Player newMasterClient)
{
  CheckTheMaster();
}
```

![Change Master](images/change-master.png)

## [Editor Test Button](https://www.udemy.com/course/unity-online-multiplayer/learn/lecture/25988020#questions)

- In Unity, add a new button called `Test Button` to your menu buttons with the label `Create Test Room`. Make sure the button is deactivated by default.
- In your `Launcher` script, add a `public GameObject roomTestButton`.
- Add a preprocessor directive to check if the game is running in the Unity Editor:

```cs
void Start()
{
  //..

  #if UNITY_EDITOR
  roomTestButton.SetActive(true);
  #endif
}
```

- Create a new public method called `QuickJoin()` in the `Launcher` script:

```cs
public void QuickJoin()
{
  RoomOptions options = new RoomOptions();
  options.MaxPlayers = MAX_PLAYERS;
  PhotonNetwork.CreateRoom("Test Room");
  CloseMenus();
  loadingText.text = "Creating Test Room";
  loadingScreen.SetActive(true);
}
```

- In Unity, assign the `QuickJoin()` function to the `Test Button's OnClick` event.
- In the `Launcher` script, assign the `Test Button` GameObject to the `roomTestButton` variable.
- Test the functionality in the Unity Editor by clicking Play. The Test Button should appear, allowing you to create and join a test room quickly.
- Build the game and verify that the Test Button does not appear in the built version of the game.

![Test Button](images/test-button.png)
