# Install Plone 6 classic UI with PDM

checkout this repository like this:

```sh
git clone https://github.com/collective/plone-classicui-pdm-installer.git plone6classicui
cd plone6classicui
```

creating a venv:

```sh
python3.11 -m venv venv
```

installing PDM with pip:

```sh
./venv/bin/pip install pdm
```
add needed Plone Add-on's in the pyproject.toml:

```toml
dependencies = [
    "Products.CMFPlone>=6.0.4",
    "plone.app.caching",
    "plone.app.iterate",
    "plone.app.upgrade",
    "Products.CMFPlacefulWorkflow",
    "collective.easyform",
    "plone.gallery",
    "collective.collectionfilter",
]
```

Install Plone and all dependencies:

```sh
./venv/bin/pdm install
```

Create a Zope instance:

```sh
./venv/bin/pdm run instance_create
```

Adjust the instance port in `instance1/etc/zope.ini`

```ini
[server:main]
use = egg:waitress#main
host = 127.0.0.1
port = 8080
```

Start the instance

```sh
./venv/bin/pdm run instance_start
```

In production you want to add this to something like supervisor or a systemd script.