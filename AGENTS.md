# Repository Guidelines

## Project Structure & Module Organization

Solaar is a Python application for managing Logitech peripherals. Application code lives in `lib/solaar/`, with GTK interfaces in `ui/` and command-line actions in `cli/`. Device protocols, descriptors, and settings live in `lib/logitech_receiver/`; supporting packages include `lib/hidapi/`, `lib/hid_parser/`, and `lib/keysyms/`.

Tests live in `tests/`, generally mirroring package structure. Documentation is in `docs/`, translations in `po/`, and icons, desktop integration, and locale assets in `share/`. Hardware permission rules live in `rules.d/` and `rules.d-uinput/`; maintenance scripts live in `tools/`.

## Build, Test, and Development Commands

Run commands from the repository root, preferably in a virtual environment:

- `python -m pip install -e '.[test,dev]'`: install editable source plus pytest and Ruff tooling. GTK, GObject, and D-Bus system dependencies must also be available; see `docs/installation.md` and the Makefile’s platform dependency targets.
  The Fedora toolbox `dev` has the `-devel` packages that a pip install compiles against: `python3-devel`, `dbus-devel`, `glib2-devel`, `gtk3-devel`, `gobject-introspection-devel` and `libX11-devel`, plus `gcc-c++` and `pkgconf-pkg-config`. Run commands in it with `toolbox run -c dev <command>`, or open a shell with `toolbox enter dev`.
- `python bin/solaar`: launch the application from the source tree.
- `python bin/solaar show`: inspect connected receivers and devices.
- `make format`: format Python code with Ruff.
- `make lint`: run Ruff checks and automatically apply fixes.
- `make test`: run pytest with coverage and write `coverage.xml`.
- `pytest tests/logitech_receiver/test_device.py`: run a focused test module.
- `pre-commit run --all-files`: run CI’s formatting and hygiene checks; install `pre-commit` separately.

## Coding Style & Naming Conventions

Use four-space indentation, `snake_case` functions and variables, `PascalCase` classes, and uppercase constants. Follow surrounding code and keep lines within Ruff’s configured 127-character limit. Ruff enforces single-line imports and checks Pyflakes, pycodestyle, bugbear, and import ordering. Keep protocol logic separate from UI presentation.

## Testing Guidelines

Use pytest, pytest-mock, and pytest-cov. Name modules `test_*.py` and functions `test_*`. Add regression coverage for behavioral changes and reuse fake devices and protocol helpers. Preserve `tests/conftest.py` isolation of user configuration and desktop notifications. Coverage includes branches and requires at least 40% overall; maintain or improve coverage for changed code.

## Commit & Pull Request Guidelines

Use concise, action-oriented commit subjects. Recent history includes optional area prefixes, such as `device: add descriptor for K380`; no mandatory Conventional Commits scheme is evident. Keep changes focused. In pull requests, explain the problem and resulting behavior, link relevant issues, and report validation. Include screenshots for visible UI changes and device/model details for hardware-specific changes.
