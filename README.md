# CRCbiome Sphinx Theme

Shared CRCbiome styling for Sphinx documentation, built on
[`sphinx_rtd_theme`](https://github.com/readthedocs/sphinx_rtd_theme).

The theme provides a consistent appearance across projects while keeping
project-specific content, logos, and configuration separate.

## Features

- CRCbiome colors and styling.
- Light and dark themes.
- Animated SVG theme toggle.
- Theme preference saved between page visits.
- System color-scheme preference used when no preference is saved.
- Responsive navigation and layout.
- Styling for code blocks, tables, admonitions, and API documentation.
- Keyboard focus indicators and reduced-motion support.

## Installation

Install a tagged release directly from GitHub:

```bash
python -m pip install \
  "crcbiome-sphinx-theme @ git+https://github.com/Rounge-lab/crcbiome-sphinx-theme.git@v0.1.0"
```

The package installs Sphinx and `sphinx-rtd-theme` as dependencies.

For reproducible documentation builds, pin to a release tag or full commit
hash rather than a development branch.

### With Pixi

Add the theme as a PyPI dependency in your documentation feature:

```toml
[feature.docs.pypi-dependencies]
crcbiome-sphinx-theme = { git = "https://github.com/Rounge-lab/crcbiome-sphinx-theme.git", tag = "v0.1.0" }
```

## Usage

Set the theme in your Sphinx `conf.py`:

```python
html_theme = "crcbiome"

html_theme_options = {
    "logo_only": True, # Optional
}
```

Sphinx discovers the installed theme automatically. There is no need to add
the theme package to `extensions`.

### Project-specific branding

Keep logos and favicons in each project's documentation as normal:

```python
html_static_path = ["_static"]

html_logo = "_static/project-logo.svg"
html_favicon = "_static/project-favicon.svg"
```

### Project-specific CSS

Additional styling can be loaded normally:

```python
html_css_files = ["project-specific.css"]
```

Place the file in the project's `_static` directory.

## Read the Docs

To use the Sphinx theme on Read the Docs, add the dependency to the
`docs/requirements.txt` or whichever file has been configured to contain the
Read the Docs build dependencies:

```text
crcbiome-sphinx-theme @ git+https://github.com/Roung-lab/crcbiome-sphinx-theme.git@v0.1.0
```

Add any other extensions required by your documentation to the same file.

Create `.readthedocs.yaml` in the consuming project's repository root:

```yaml
version: 2

build:
  os: ubuntu-24.04
  tools:
    python: "3.12"

sphinx:
  configuration: docs/source/conf.py
  fail_on_warning: true

python:
  install:
    - requirements: docs/requirements.txt
    - method: pip
      path: .
```

## Theme behavior

On the first visit, the theme follows the browser's
`prefers-color-scheme` setting.

The toggle switches between light and dark mode and saves the selected
theme in browser local storage under `crc-theme`. If local storage is
unavailable, the toggle still works, but the preference is not persisted.

The icon represents the current theme:

- Sun: light theme.
- Moon: dark theme.

The button tooltip describes the action that clicking it will perform.

Preferences are stored per website origin, so documentation hosted on the
same origin may share the saved preference.

## Development

Clone the repository and install the package in editable mode:

```bash
git clone https://github.com/Rounge-lab/crcbiome-sphinx-theme.git
cd crcbiome-sphinx-theme

python -m pip install -e .
```

Install it into a consuming project's documentation environment, then build
that project's documentation:

```bash
python -m sphinx -b html docs/source docs/_build/html
```

To check the installed base theme version:

```bash
python -c "from importlib.metadata import version; print(version('sphinx-rtd-theme'))"
```

With Pixi:

```bash
pixi run -e docs python -c "from importlib.metadata import version; print(version('sphinx-rtd-theme'))"
```

### Testing changes

Before releasing, check:

- Light and dark appearance.
- Theme switching and persistence across page navigation.
- Mobile and desktop navigation.
- Nested navigation sections.
- Code blocks and syntax highlighting.
- Tables, admonitions, and directive signatures.
- Keyboard navigation and visible focus.
- Reduced-motion behavior.
- Compatibility with the supported Sphinx and `sphinx-rtd-theme` versions.

## Releases

1. Test the theme against a representative documentation project.
2. Update the version in `pyproject.toml`.
3. Commit the changes.
4. Create and push a matching release tag, such as `v0.1.0`.
5. Update consuming projects when they are ready to adopt the release.

Pinned releases let projects upgrade independently without unexpected
styling changes.
