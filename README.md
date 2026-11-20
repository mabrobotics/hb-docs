# hb-docs

Documentation for Honey Badger robots.

## Documentation structure

- [Getting Started](GettingStarted/GettingStarted.md) — robot introduction and key specs
- [First Startup](GettingStarted/FirstStartup.md) — first startup checklist
- [Safety](GettingStarted/Safety.md) — safety and emergency procedures
- [Setup](GettingStarted/Setup.md) — package contents, network reference, charging
- [Control Console](GettingStarted/ControlConsole.md) — Steam Deck controller and operator console
- [Physical Overview](Physical/Overview.md) — mechanical overview
- [Connectors](Physical/Connectors.md) — connectors, buttons, and port reference
- [Software Overview](Software/Overview.md) — ROS2 architecture and core topics
- [Platform](Software/Platform.md) — onboard LPC/APC platform roles
- [Messages](Software/Messages.md) — custom ROS2 message reference
- [Networking](Software/Networking.md) — Wi-Fi, SSH, and ROS2 network notes
- [Geometry](Software/Geometry.md) — coordinate frames and leg geometry
- [Payload](Payload/Payload.md) — payload mount, spool, camera and lidar configuration

## Building

### Prepare build environment

```bash
sudo apt install python3-venv
python3 -m venv venv
. venv/bin/activate
pip install -r requirements.txt
```

### Build the docs

```bash
jupyter-book build .
```

### PDF of a released version

Before the docs move to a new software version, build the PDF of the current one and add it to `downloads/` and the list in [intro.md](intro.md). Put the version in `title` in `_config.yml` first, then:

```bash
jupyter-book build --all --builder pdflatex .
gs -q -sDEVICE=pdfwrite -dPDFSETTINGS=/printer -dNOPAUSE -dBATCH -sOutputFile=downloads/hb-docs-v<version>.pdf _build/latex/book.pdf
```

Needs LaTeX (`latexmk`, `xelatex`) and Ghostscript. Revert the `title` change afterwards.

## Pushing & Merging

Before every merge, do a typos check with:

```bash
typos
```
