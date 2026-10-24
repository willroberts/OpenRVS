# OpenRVS
A patch to fix Red Storm Entertainment's mistakes (intentional and otherwise). Allows multiplayer again, enables more serious modding, implements some QOL fixes.

## Installation Instructions
1. Download the latest release ZIP from the
[Releases page](https://github.com/OpenRVS-devs/OpenRVS/releases).
1. Stop your game if it's running.
2. Extract the contents of the OpenRVS ZIP file to your game's `system`
   directory.
3. Optionally adjust your FOV in `openrvs.ini` - see the `[OpenRVS.OpenFOV]`
   section.
4. Launch the game! You'll see a list of servers to choose from, or you can join
   by IP address.

## How It Works
- Replaces the old Ubi.com server list with a community server list.
- Overrides the client's network data rate to 100KB/s. Players no longer need to select T1/T3 in network settings.

## Installing or Updating OpenRVS (Server Instructions)
1. Download the latest release ZIP from the
[Releases page](https://github.com/OpenRVS-devs/OpenRVS/releases).
2. Stop your server process if it's running.
3. Extract the contents of the OpenRVS ZIP file to your game's `system`
   directory.
4. Edit `RavenShield.mod` in your game's `Mods` directory, and add the following
   lines. NOTE: The `OpenRVS.OpenBeacon` line should replace the
   `IpDrv.UdpBeacon` line. This may not be compatible with N4Admin installations.
```ini
ServerActors=OpenRVS.OpenServer
ServerActors=OpenRVS.OpenBeacon
ServerActors=OpenRenderFix.OpenFix
ServerPackages=OpenRenderFix
```
   Apply these settings to listen hosts started through the in-game menu too.
   On listen servers only, `OpenRVS.OpenServer` replaces the host's local
   `OpenRVS.OpenHUD` with `OpenRenderFix.OpenRender`;
   `ServerPackages=OpenRenderFix` makes that HUD
   package available to joining clients. Keep both settings together. HUDs
   supplied by other mods are preserved.
5. Edit `system/RavenShield.ini`, and ensure that your `ServerBeaconPort` is set
   to exactly 1000 above your server's `Port`, and that `BeaconPort` is exactly
   2000 above your server's `Port`.
6. Launch the server!

NOTE: Server admins can ban users by IP address in the `BanList.ini` file, found
in your game's `Server` directory.

## Patching R6GameService.dll
The `R6GameService.dll` file contains code which makes requests to Ubisoft's
historical infrastructure for the game, which is now shut down. This can result
in lag or stutter.

To fix this, you can patch the file using
[ChrisWak's patcher](https://github.com/eth0up/R6GameServicePatcher), or simply
replace the file with an
[already-patched version](https://github.com/willroberts/raven-shield-2020/blob/main/R6GameService.dll).

## The OpenRVS Registry
Since v1.5, OpenRVS servers send an extra UDP beacon to a web server running
[openrvs-registry](https://github.com/willroberts/openrvs-registry). This app tracks
all known servers, healthchecks them to hide unhealthy serers, and automatically
adds new servers to the list when the UDP beacon is received.

Since v1.6, the config key is `RegistryServerHost` instead of `RegistryServerIP`,
and DNS domain names are supported (rather than just IP addresses).

The registry listens for beacons on UDP port 8080, and listens for server list
requests on HTTP port 8080 (on the /servers URL).

The IP and port for the deployment can be configured in `openrvs.ini`:

```ini
[OpenRVS.OpenBeacon]
RegistryServerHost=api.openrvs.org
RegistryServerPort=8080
```

## Local Development
See [CONTRIBUTING.md](CONTRIBUTING.md).

## Version History
See [CHANGELOG.md](CHANGELOG.md).

## Credits
By the OpenRVS team:
  - original author: Twi
  - programming: Twi and Will
  - game service fix: chriswak
  - community hosting: Tony and SMC Clan

Thanks to: Tony, Psycho, Legacy, juguLator01, SMC and ShadowSquad clans, and the AllR6 community.

## License
GPL v3
