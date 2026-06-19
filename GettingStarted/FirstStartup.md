# First startup instructions

This page gives a minimal, safe quick-start to get a new Honey Badger 5.0 powered on, connected, and standing.

> Target audience: operators who need a short, reliable checklist to bring the robot to an operational standing state.

## Quick checklist (read before starting)

- Read the Safety section in the main manual.
- Place robot on flat, stable ground with at least 1 m clearance around legs.
- Ensure emergency stop (E-stop) is released and accessible.
- Use two people to lift or position the robot if needed.
- Have the control console (Steam Deck) or laptop ready.

## 1 — Power on (approx. 90 s)

1. Place the robot on level ground and make sure nothing blocks leg motion.
2. Press and hold the robot power button for 2 seconds.
3. Wait for the power-up sequence to complete (~90 s). Watch status LEDs.

## 2 — Connect control console and network

- On the Steam Deck (or laptop): connect to the robot's AP and open the control GUI.
- Confirm the control console shows the robot as `connected`.
- For the AP name, password, and wired connection options, see [Setup](Setup.md).

## 3 — Verify system status (basic checks)

- Check battery level is sufficient (> 30% recommended for initial testing).
- Confirm ROS2/bridge status if shown in the GUI (nodes should be `OK`).
- Look for any error or warning messages in the console/log window.

## 4 — Energize actuators (enable)

1. Ensure area is clear and everyone stands back.
2. From the control console select the `ENERGIZE` (enable) command.
3. If a hardware E-stop is present and active, reset it only when safe.

For full safety guidance, see [Safety](Safety.md).

## 5 — Stand up sequence

1. With actuators energized, use the `STAND UP` command in the GUI.
2. Wait until the robot reaches a stable standing posture.
3. Switch to `Stand` or `Active Balance` mode as required by your workflow.
4. If the robot does not stand or joints twitch, immediately command `IDLE` and disable actuators.

## 6 — Basic drive test (low risk)

- Select a slow gait (e.g., `Stable Walk`) and command a small forward step.
- Observe leg motion and listen for abnormal noises.
- If behavior is abnormal: immediately stop, select `IDLE`, and disable actuators.

## 7 — Shutdown

1. Stop all motion and select `IDLE` in the GUI.
2. Wait until the robot is stable and logs show no active motion.
3. Press and hold the power button for 2 seconds to power off.

## Troubleshooting (quick)

- No Wi‑Fi: check that AP SSID is broadcasting; try Ethernet if available.
- Control console says disconnected: reboot the console app and re-check Wi‑Fi.
- Robot fails to energize: check E‑stop, battery voltage, and error logs in the console.
- Overheating or warning LEDs: disable actuators and inspect hardware.

## Next steps / Links

- For full setup and charging procedures see [Setup](Setup.md).
- For detailed control console usage see [ControlConsole](ControlConsole.md).
- For safety and emergency procedures see [Safety](Safety.md).
