# Software Update

The robot software is delivered by MAB as Debian packages, one for each device:

| Package | Device |
| --- | --- |
| `hb50-robot_<version>_amd64.deb` | Robot (LPC) |
| `hb50-remote_<version>_amd64.deb` | Steam Deck |
| `hb50-payload_<version>_arm64.deb` | APC, only with the camera and lidar payload, see [Camera & Lidar](../Payload/CameraLidar.md) |

The software is installed in `/opt/<package name>`. The robot and the Steam Deck must run the same version; otherwise the status console shows the warning `Robot runs v<version>, remote v<version>: update both to the same version`.

```{note}
This chapter covers the robot's ROS 2 software stack running on the LPC and the `HB50 Remote` application on the Steam Deck. It is not related to the mainboard firmware update mode described in {ref}`Button & Connectors <firmware-update>`.
```

```{note}
Robots and Steam Decks running v1.2.0 or older have to be prepared once before installing the package, see {ref}`Moving from v1.2.0 or older <moving-from-v1-2>`.
```

## Updating the robot

```{warning}
Installing the package restarts the robot software. Lay the robot down in a stable position, select `IDLE` and make sure the robot stays powered on during the installation (connect the charger if the battery is low).
```

1. Connect a laptop or the Steam Deck to the robot's network (see [Network](Networking.md)) and copy the package to the robot:

   ```bash
   scp hb50-robot_<version>_amd64.deb hb@10.11.0.10:
   ```

2. Connect to the robot over SSH and install the package:

   ```bash
   ssh hb@10.11.0.10
   sudo apt install ./hb50-robot_<version>_amd64.deb
   ```

The package restarts the `hb50` service, so the new version runs right after the installation.

## Updating the Steam Deck

1. Copy the package to the Steam Deck, for example from a USB stick in the Steam Deck desktop mode.
2. Close the `HB50 Remote` application, open a terminal in the folder with the package and install it:

   ```bash
   sudo apt install ./hb50-remote_<version>_amd64.deb
   ```

3. Start the `HB50 Remote` application with the `HB50 Remote` icon on the desktop.

If `apt` reports missing dependencies, connect the device to the internet and run the command again.

## Checking the installed version

On the robot or the Steam Deck, run:

```bash
dpkg-query -W hb50-robot     # on the robot
dpkg-query -W hb50-remote    # on the Steam Deck
```

The robot version is also published on the `/hb50/version_data/robot` topic, see [VersionData](Messages.md).

## Settings

Package updates keep changed files in `/etc`, so the settings below are set once:

| File | Device | Contents |
| --- | --- | --- |
| `/etc/default/hb50` | Robot | Robot name (`ROBOT_NS`: `hb50`, `hb50w` or `hb40`), `ROS_DOMAIN_ID` and the network interface (`NETWORK`). Applied with `sudo systemctl restart hb50`. |
| `/etc/default/hb50-remote` | Steam Deck | Name of the robot to control (`ROBOT_NS`) and `ROS_DOMAIN_ID`. Applied by restarting the `HB50 Remote` application. |

`ROBOT_NS` and `ROS_DOMAIN_ID` must be the same on the robot and the Steam Deck.

## Going back to an older version

Install the older package with:

```bash
sudo apt install --allow-downgrades ./hb50-robot_<older version>_amd64.deb
```

On the Steam Deck, use the `hb50-remote` package in the same way. Keep the robot and the Steam Deck on the same version.

(moving-from-v1-2)=
## Moving from v1.2.0 or older

Software v1.2.0 and older ran from `~/install` and was updated with update bundles. Remove it once before installing the package. Saved parameters and maneuvers in `~/.ros/mab` are kept.

### Robot

Connect to the robot over SSH (user `hb`) and remove the old software:

```bash
sudo systemctl disable --now hb50.service
sudo rm -f /etc/systemd/system/hb50.service /etc/systemd/system/hb50-updater.service /etc/sudoers.d/hb50-updater
sudo systemctl daemon-reload
rm -rf ~/install ~/install-* ~/hb50-tools ~/updates
```

Set the robot name, replacing `<robot name>` with `hb50`, `hb50w` (robot with wheels) or `hb40`:

```bash
sed -i -e '/install\/setup.bash/d' -e '/HB50_NAMESPACE/d' ~/.bashrc
printf 'ROBOT_NS=<robot name>\nROS_DOMAIN_ID=69\nNETWORK=enp1s0\n' | sudo tee /etc/default/hb50
```

Install the package, keeping the robot name set above:

```bash
sudo apt install -o Dpkg::Options::=--force-confold ./hb50-robot_<version>_amd64.deb
```

### Steam Deck

Open a terminal on the Steam Deck (user `mab`), remove the old software and install the package:

```bash
rm -rf ~/install ~/install-* ~/updates ~/Desktop/hb.desktop
sed -i '/install\/setup.bash/d' ~/.bashrc
sudo apt install ./hb50-remote_<version>_amd64.deb
```

Set the name of the robot to control, replacing `<robot name>` as above:

```bash
sudo sed -i 's/^ROBOT_NS=.*/ROBOT_NS=<robot name>/' /etc/default/hb50-remote
```

Start the `HB50 Remote` application with the `HB50 Remote` icon on the desktop.
