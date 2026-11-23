# hb-docs

Documentation for Honey Badger robots.

## Documentation structure

- [Intro](intro.md) — key specs, PDFs for older software versions
- [Getting Started](GettingStarted/GettingStarted.md) — robot introduction
- [Safety](GettingStarted/Safety.md) — safety and emergency procedures
- [First Startup](GettingStarted/FirstStartup.md) — first startup checklist
- [Setup](GettingStarted/Setup.md) — package contents, network reference, charging and charger/battery connections
- [Control Console](GettingStarted/ControlConsole.md) — Steam Deck controller and operator console
- [Physical Overview](Physical/Overview.md) — mechanical overview
- [Connectors](Physical/Connectors.md) — connectors, buttons, and port reference
- [Software Overview](Software/Overview.md) — ROS2 architecture and core topics
- [Platform](Software/Platform.md) — onboard LPC/APC platform roles
- [Messages](Software/Messages.md) — custom ROS2 message reference
- [Networking](Software/Networking.md) — SSH and ROS 2 network notes
- [Geometry](Software/Geometry.md) — coordinate frames and leg geometry
- [Software Update](Software/Updating.md) — robot and Steam Deck updates
- [Payload](Payload/Payload.md) — payload mount
- [Camera & Lidar](Payload/CameraLidar.md) — camera and lidar payload
- [Spool](Payload/Spool.md) — spool payload
- [FAQ](FAQ.md) — frequently asked questions

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
