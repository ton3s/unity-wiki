# Photon PUN v2

## Architecture

Everyone connects to a "Name Server" first. It checks which app (with the AppId) and which region the client wants to use. Then it forwards the client to a Master Server.

The Master Server is the hub for a bunch of regional servers. It knows all rooms for this region. Any time a room (match / game) gets created or joined, the client gets forwarded to one of the other machines, called "Game Server".

## Tips

- Best way to resuse `GameObjects` is to turn them into `prefabs`
- `Prefabs` that should get instaniated over the network 