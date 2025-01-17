# pmms - FiveM/RedM synchronized media player

pmms (Poodle's MultiMedia System) allows players to play music/video from entities such as phonographs, radios or TVs.

# Features

- NUI-based, using [MediaElement.js](https://www.mediaelementjs.com/) to support HTML5 media, HLS, YouTube, and more.

- Synchronized between players.

- Multiple entities can play different things at the same time.

- Dynamic sound attenuation based on whether the player and entity are in the same interior room.

- Optional immersive filter can be applied to any audio.

- Play video on a TV screen with DUI (FiveM), or on a screen displayed above the entity (RedM).

- Permissions system and ability to lock entities so only certain players can control them.

- Configure default entities which are spawned and play music/video automatically.

- Audio visualizations via [Wave.js](https://foobar404.github.io/Wave.js/#/).

# Examples

| | | |
|-|-|-|
|[![Attenuation Example](https://i.imgur.com/BTkglVYm.jpg)](https://imgur.com/BTkglVY)| [![Phonograph Filter](https://i.imgur.com/L8sWpOCm.jpg)](https://imgur.com/L8sWpOC) | [![Video](https://i.imgur.com/2jRYlSem.jpg)](https://imgur.com/2jRYlSe) |
|[![FiveM basic audio](https://i.imgur.com/CofS0VPm.jpg)](https://imgur.com/CofS0VP)|[![FiveM DUI example](https://i.imgur.com/ndZwPvDm.jpg)](https://imgur.com/ndZwPvD)|[![DUI render target proximity](https://i.imgur.com/m2KddI6m.jpg)](https://imgur.com/m2KddI6)|
| |[![Audio Visualizations](https://i.imgur.com/4E42m4tm.jpg)](https://imgur.com/4E42m4t)| |

# Installing

1. Place the files from this repository in a new folder in your resources directory.

   Example: `resources/[local]/pmms`
   
   > **NOTE**
   > 
   > The name of the resource **must** be in all lowercase in order for it to function properly. This is due to how [NUI callbacks](https://docs.fivem.net/docs/scripting-manual/nui-development/nui-callbacks/) work.

2. Add the following in server.cfg:
   ```
   exec resources/[local]/pmms/permissions.cfg
   start pmms
   ```
   
   Adjust as necessary based on where you installed the resource and what you named it.

# Commands

> **Note**
> 
> The command names can be customized. These are the defaults.

| Command                | Description                                       |
|------------------------|---------------------------------------------------|
| `/pmms`                | Open the media player control panel.              |
| `/pmms_play [url] ...` | Play music/video on the nearest media player.     |
| `/pmms_pause`          | Pause playback on the nearest media player.       |
| `/pmms_stop`           | Stop playback on the nearest media player.        |
| `/pmms_status`         | Show the status of the nearest media player.      |
| `/pmms_presets`        | List presets.                                     |
| `/pmms_vol [volume]`   | Set a personal base volume for all media players. |
| `/pmms_fix`            | Reset your client, which may fix certain issues.  |
| `/pmms_ctl`            | Advanced media player control.                    |
| `/pmms_add`            | Add or modify a media player model preset.        |
| `/pmms_refresh_perms`  | Refresh permissions for all clients.              |

# Exports

## Server-side

### startByNetworkId

```lua
handle = exports.pmms:startByNetworkId(netId, options)
```

Starts playing something on a networked media player entity, using its network ID.

#### Media player options

`options` is a table of options for the new media player.

| Option           | Description                                                                             |
|------------------|-----------------------------------------------------------------------------------------|
| `url`            | The URL of the media to play.                                                           |
| `title`          | The title of the media to display.                                                      |
| `volume`         | The volume of the new media player. Default: 100.                                       |
| `offset`         | The time to start the media at in seconds. Default: 0.                                  |
| `duration`       | The duration of the media. `nil`, `false` or `0` will treat the media as a live stream. |
| `loop`           | Whether to loop the media. Requires a duration.                                         |
| `filter`         | Whether to apply the immersive filter to the media player.                              |
| `locked`         | Whether to lock the media player.                                                       |
| `video`          | Whether to display NUI video (RedM only).                                               |
| `videoSize`      | The size of the NUI video screen (RedM only).                                           |
| `muted`          | Whether the media player is muted by default.                                           |
| `attenuation`    | The attenuation multipliers for the media player.                                       |
| `diffRoomVolume` | Difference between the base volume in the same room and a different room.               |
| `range`          | The range of the media player.                                                          |
| `visualization`  | The audio visualization to apply to the media player.                                   |

### startByCoords

```lua
handle = exports.pmms:startByCoords(x, y, z, options)
```

Starts playing something on a non-networked media player entity, using its coordinates on the world map.

Refer to [Media player options](#media-player-options).

### stop

```lua
exports.pmms:stop(handle)
```

Stops a media player and removes its handle.

### pause

```lua
exports.pmms:pause(handle)
```

Pause or resume a media player.

### lock

```lua
exports.pmms:lock(handle)
```

Locks an active media player so that only privileged users can interact with it.

### unlock

```lua
exports.pmms:unlock(handle)
```

Unlocks an active media player so anyone can interact with it.

### mute

```lua
exports.pmms:mute(handle)
```

Mutes an active media player.

### unmute

```lua
exports.pmms:unmute(handle)
```

Unmutes an active media player.

## Client-side

### enableEntity

```lua
exports.pmms:enableEntity(entity)
```

Allows the player to use a particular entity if they do not have the `pmms.anyEntity` ace.


### disableEntity

```lua
exports.pmms:disableEntity(entity)
```

Disables the use of this entity for a player without the `pmms.anyEntity` ace.


### createMediaPlayer

```lua
entity = exports.pmms:createMediaPlayer(options)
```

Creates a new networked entity which the player can use as a media player.

#### createMediaPlayer options

| Option     | Description                                                              |
|------------|--------------------------------------------------------------------------|
| `model`    | The entity model to use. If omitted, `Config.defaultModel` will be used. |
| `position` | A vector3 for the coordinates where the entity will be spawned.          |
| `rotation` | A vector3 for the orientation of the entity.                             |


### deleteMediaPlayer

```lua
exports.pmms:deleteMediaPlayer(entity)
```

Deletes an entity created by [createMediaPlayer](#createmediaplayer).
