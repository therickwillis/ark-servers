# ark-servers
repo for creating and managing ark servers. Currently focused on using LinuxGSM to manage.

# Cluster

## Installation

Note that all server commands documented here will assume the `./arkserver` default alias though this may differ on unique cluster instances.

## Configuration

### arkserver.cfg

Quick Edit: `nano lgsm/config-lgsm/arkserver/arkserver-lostisland.cfg`

The `arkserver.cfg` file is located in `./lgsm/config-lgsm/arkserver/`.  The name correspondes with the name of the installation script.  The default is `arkserver`.  These values must be different tor un on the same machine.

Use a port map to manage the ports for each instance in your cluster.  None of the ports can be shared.  Steam reserves some ports too so don't step on those (todo:// get those ports). 

*Note that the Port number will is incremented by 1 and used by Ark. 7777 also reserves 7778.*

| Map | Port | QueryPort | RCONPort |
| -- | -- | --| -- |
| The Island | 7777 | 27015 | 27020 |
| Lost Island | 7779 | 27016 | 27021 |

```
# Ports
port="7777"
queryport="27015"
rconport="27020"

# Map
# TheIsland, Ragnarok, CrystalIsles, Aberration_P, ScorchedEarth_P, TheCenter, Extinction, Valguero_P, Genesis, LostIsland
defaultmap="CrystalIsles"

# Other Settings
maxplayers=70
clusterid="FOADCluster"

# Start Command Params
#  - this should be templated for values
#    - i'm aware that it currently isn't
startparameters="${defaultmap}?SessionName=FOAD ${defaultmap} Server?AltSaveDirectoryName=${defaultmap}?listen?MultiHome=${ip}?MaxPlayers=${maxplayers}?QueryPort=${queryport}?RCONPort=${rconport}?Port=${port} -automanagedmods -NoTransferFromFiltering -clusterid=${clusterid}"

```

### GameUserSettings.ini

Folder: `./serverfiles/ShooterGame/Saved/Config/LinuxServer/`

Quick Command: `nano serverfiles/ShooterGame/Saved/Config/LinuxServer/GameUserSettings.ini`

The `GameUserSettings.ini` file is located in `./serverfiles/ShooterGame/Saved/Config/LinuxServer`.  This file defines values for the server configuration.  It can be found using `./arkserver details`. There are multiple sections.  Here are the values to change.

| Setting | Value | Section | Notes |
| -- | -- | -- | -- |
| AllowThirdPersonPlayer | True | ServerSettings |
| EnablePvPGamma | True | ServerSettings |
| DifficultyOffset | 1.0 | ServerSettings |
| OverrideOfficialDifficulty | 5.0 | ServerSettings |
| RCONPort | *RCONport from arkserver.cfg* | ServerSettings | Be sure to set this to match the `arkserver.cfg` setting. May be easier to set RCONEnable=False and not deal with it. |
| XPMultiplier | 3.0 | ServerSettings | |
| HarvestAmountMultiplier | 3.0 | ServerSettings | |
| TamingSpeedMultiplier | 3.0 | ServerSettings | |


Add these to the bottom of the ServerSettings

```
OverrideOfficialDifficulty=5.0
XPMultiplier=3.0
HarvestAmountMultiplier=3.0
TamingSpeedMultiplier=3.0
```

### Game.ini

Folder: `./serverfiles/ShooterGame/Saved/Config/LinuxServer`

This file is empty by default.  Add the following settings.

```
[/script/shootergame.shootergamemode]
MatingIntervalMultiplier=0.5
EggHatchSpeedMultiplier=4.0
BabyMatureSpeedMultiplier=4.0
BabyImprintAmountMultiplier=4.0
HexagonRewardMultiplier=1.5
BabyCuddleIntervalMultiplier=0.6
```

## Notable Cluster Issues

| Symptom | Reason |
| --- | --- |
| 2nd server starts with 0/0 players | The server didn't start completely.  Probably related to ports or shared files (of which there should be none). |
| Server Transfer failed to bring character to destination server | By default, the cluster folder is unique to the instance of the server.  To fix this, create a shared folder for housing cluster files. Then create a symlink to the folder in all of the Ark instance servers.  This is in `ShooterGame/Saved/cluser` |

## Scripts

Need a script for the initial config

Incoming Params
* map
* port
* queryport
* rconport

