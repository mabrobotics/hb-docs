# Software Update

The robot software can be updated in the field using a signed update bundle supplied by MAB. The update is started from the [Steam Deck controller](../GettingStarted/ControlConsole.md) and updates both the robot and the Steam Deck. It does not require rebuilding anything.

```{note}
This chapter covers the robot's ROS 2 software stack running on the LPC and the `HB50 Remote` application on the Steam Deck. It is not related to the mainboard firmware update mode described in {ref}`Button & Connectors <firmware-update>`.
```

## Update bundle

Updates are delivered as a single file named `hb50-<target>-v<version>.tar`, for example `hb50-all-v1.3.0.tar`. Target `all` means the bundle is valid for both the robot and the Steam Deck. The file can be sent by email, downloaded or copied from a USB stick — do not unpack or rename it.

Every bundle is signed by MAB. Before installing, both the Steam Deck and the robot verify:

- the signature against the MAB vendor key built into the software,
- the integrity (SHA-256) of the software package inside the bundle,
- that the bundle is intended for the device (target `robot`, `steamdeck` or `all`),
- that the bundle version is newer than the installed one.

A bundle that fails any of the first three checks is rejected and nothing is changed.

```{note}
Robots and Steam Decks running software older than v1.2.0 must be updated once with the {ref}`first update <first-update>` package before they can use the procedure below.
```

## Updating the robot

### 1. Copy the bundle to the Steam Deck

Copy the bundle into the `updates` folder in the home directory of the Steam Deck (`/home/mab/updates/`), for example from a USB stick in the Steam Deck desktop mode, or from a laptop connected to the robot's network (see [Network](Networking.md)):

```bash
scp hb50-all-v1.3.0.tar mab@deck.lan:~/updates/
```

If there are several bundles in the folder, the one with the newest version is used.

### 2. Put the robot in a safe state

Connect the Steam Deck to the robot's network. Updates are only accepted when the robot is in the `idle` motion mode. Lay the robot down in a stable position and make sure it stays powered on for the whole procedure (connect the charger if the battery is low).

### 3. Check the update

In the `HB50 Remote` application, use the {ref}`Command Switch <user-interface>` to go to the Power section and press `CHECK SOFTWARE UPDATE`.

The Steam Deck verifies the bundle, copies it to the robot (`/home/hb/updates/`) and asks the robot to verify it as well. The result is shown in the status console, with the `Remote:` part for the Steam Deck and the `Robot:` part for the robot:

- white (OK) — the bundle is verified and ready to install, the console shows its version and git revision,
- yellow (warning) — the bundle is valid, but it is not newer than the installed version; applying it will reinstall or downgrade the software,
- red (error) — the bundle was rejected, see {ref}`Status codes <status-codes>`.

### 4. Apply the update

Press `APPLY SOFTWARE UPDATE`. This button only works after a successful check.

The robot is updated first:

1. it unpacks the new software next to the current one (`/home/hb/install-<version>-<id>/`),
2. stops the robot software (`hb50` service),
3. switches `/home/hb/install` to the new version,
4. starts the robot software again and waits up to 60 s for the first control cycle, which shows that the configuration, the hardware communication and the controller all work.

If the new software starts correctly, the bundle is removed from `/home/hb/updates/` and older software versions are deleted, keeping only the current and the previous one. If it does not start, the robot automatically rolls back to the previous version.

The Steam Deck waits up to 3 minutes for the robot to come back with the new version and then installs the same version on itself, in the same way. This takes about 30 seconds in total.

```{warning}
The robot software restarts during the update, so the Steam Deck loses connection to the robot for a moment. Do not power off the robot or close the `HB50 Remote` application until the update has finished.
```

### 5. Restart the application

When the status console shows `restart the app to finish the update`, close the `HB50 Remote` application and start it again.

If the console shows `Robot did not come back with v<version> - remote was NOT updated, check the robot`, the robot update failed and the Steam Deck was not changed. The updater log can be checked on the robot with:

```bash
journalctl -u hb50-updater
```

To confirm the installed version, connect over SSH to the robot or the Steam Deck and run:

```bash
cat ~/install/.hb50_version
```

When the robot and the Steam Deck run different versions, the status console shows the warning `Robot runs v<version>, remote v<version>: update both to the same version`.

(status-codes)=
## Status codes

Status codes returned by `CHECK SOFTWARE UPDATE` (the Steam Deck reports only the message, in the `Remote:` part):

| Code | Name | Meaning |
| --- | --- | --- |
| 0 | `OK` | Bundle verified and ready to install |
| 1 | `ERR_BUNDLE_NOT_FOUND` | No `hb50-<target>-v<version>.tar` file for the device in `~/updates/` |
| 2 | `ERR_EXTRACT_FAILED` | The file is not a valid bundle |
| 3 | `ERR_MISSING_FILES` | The bundle is incomplete or corrupted |
| 4 | `ERR_INVALID_SIGNATURE` | The bundle is not signed by MAB — refused |
| 5 | `ERR_MANIFEST_PARSE` | The bundle metadata could not be read |
| 6 | `ERR_WRONG_TARGET` | The bundle is not intended for the device |
| 7 | `ERR_PAYLOAD_HASH_MISMATCH` | The software package was modified or damaged — refused |
| 8 | `WARN_NOT_NEWER_VERSION` | The bundle is not newer than the installed version |
| 9 | `ERR_ROBOT_NOT_IDLE` | The robot is not in `idle` motion mode |

If there is no bundle on the Steam Deck, only the robot is checked, using a bundle already present in `/home/hb/updates/` on the robot. The Steam Deck is then not updated.

If the console shows `Could not copy <bundle> to the robot`, the Steam Deck could not reach the robot over SSH. Check the network connection. The Steam Deck logs in to the robot with an SSH key set up by the {ref}`first update <first-update>`; if the key is missing, run `ssh-copy-id hb@10.11.0.10` on the Steam Deck.

Status codes returned by `APPLY SOFTWARE UPDATE`:

| Code | Name | Meaning |
| --- | --- | --- |
| 0 | `OK` | Update handed over to the updater, the robot software will restart |
| 1 | `ERR_NO_VERIFIED_UPDATE` | No successful check was run before applying |
| 2 | `ERR_UNPACK_FAILED` | The new software could not be unpacked (e.g. no disk space) |
| 3 | `ERR_DEPLOY_FAILED` | The updater could not be started |
| 4 | `ERR_ROBOT_NOT_IDLE` | The robot is not in `idle` motion mode |

(first-update)=
## First update

The first update uses the package `hb50-first-update-v<version>.tar` supplied by MAB, which contains the update bundle and the scripts that install it on both devices.

1. Unpack the package on the Steam Deck.
2. Put the robot in `idle` mode, connect the Steam Deck to the robot's network and close the `HB50 Remote` application.
3. Open a terminal on the Steam Deck in the unpacked folder and run:

   ```bash
   ./first_update.sh hb50-all-v<version>.tar
   ```

   If the robot does not use the default address `10.11.0.10`, add it at the end, for example `./first_update.sh hb50-all-v<version>.tar hb@<robot-ip>`.

4. Type the robot password when asked. You may be asked twice: once to set up the SSH key, once to install the software. If the script asks for the robot name, type the model: `hb40`, `hb50`, or `hb50w` for the robot with wheels.
5. Wait until the script prints `=== Done: robot and remote run v<version>. Start the HB50 Remote app again.`
6. Start the `HB50 Remote` application with the `HB50 Remote` icon on the desktop. The script creates the icon for your robot; the old `Honey Badger` icon no longer works and is removed.

The script can be run again safely: the robot or the Steam Deck is skipped if it already runs this version. If the robot is not reachable, only the Steam Deck is updated and the script ends with `robot was NOT updated` — connect the robot and run the same command again. If something fails, the script stops and prints the reason; if it stops before `Stopping hb50...`, nothing on the robot was changed.

The software installed before the first update is kept in `~/install-legacy` on both devices.

## ROS 2 services

The robot update can also be triggered without the Steam Deck, using the services provided by `control_node`. This updates only the robot, from a bundle copied to `/home/hb/updates/` on the robot:

| Service | Type | Description |
| --- | --- | --- |
| `/hb50/check_for_update` | `hb50_commons/srv/CheckForUpdate` | Verifies a bundle. `bundle_path` selects the bundle file or the directory to search (empty — `/home/hb/updates/`) |
| `/hb50/apply_update` | `hb50_commons/srv/ApplyUpdate` | Installs the bundle verified by the last check |

```bash
ros2 service call /hb50/check_for_update hb50_commons/srv/CheckForUpdate "{bundle_path: ''}"
ros2 service call /hb50/apply_update hb50_commons/srv/ApplyUpdate
```

## Manual rollback

To go back to the previous version on the robot, connect over SSH and run:

```bash
ls -d ~/install-*                       # list installed versions (install-legacy is the version before the first update)
sudo systemctl stop hb50
ln -sfn ~/install-<previous_version> ~/install
sudo systemctl start hb50
```

On the Steam Deck, run the same `ls` and `ln` commands, then restart the `HB50 Remote` application. Keep the robot and the Steam Deck on the same version.
