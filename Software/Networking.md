# Network

Robot is equipped with on-board WiFi router, allowing for easy access, updates and managing multiple
connected devices.

By default, the robot is configured with independent WiFi access point. SSID follows a pattern:
- **hb50_XXXX** - for 2.4Ghz network,
- **hb50_XXXX_5G** - for 5Ghz network.

Where XXXX is robot serial code shortened to 4 hex symbols - e.g. "hb50_AFAB" or "hb50_D71C_5G".
Default password to the network is: `milkaorzechowa`.

The router also hosts DNS server, allowing for easy SSH connections.

```{figure} ./img/network.png
:alt: network
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

```{note}
Due to ROS2 constrains, when a node (such as `hb_remote`) is launched *before* network connection
is estabilished (IP address resolved), it usually fails to properly discover and detect other ROS2 nodes,
topics and services.

Before starting any node that will interoperate with Honey Badger, make sure you are connected to correct 
network.
```

## ROS2 and SSH
Both of the robots on-board computers can be connected to via ROS2 or direcly with ssh. 

### SSH 
Robots' DNS server uses 10.11.0.1 as base address (also Router address), and both LPC and APC have static
IP address:
| Device | Static Address | Hostname | Username |
| --- | --- | --- | ---|
| LPC | 10.11.0.10 | hb50.lan | hb |
| APC (Orin) | 10.11.0.11 | hb50-orin.lan | hb-orin |
| SteamDeck | Dynamic IP | deck.lan | mab |

### ROS2
By default, robots use ROS_DOMAIN_ID of 69, thus robots topics should be visible shortly after power-up.
No further customization is required.

