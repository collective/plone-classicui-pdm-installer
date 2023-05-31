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
    ### only needed when using relstorage
    "relstorage",
    "psycopg2",
    ###
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
./venv/bin/pdm run create_instance
```

Adjust the instance port in `instance1/etc/zope.ini`

```ini
[server:main]
use = egg:waitress#main
host = 127.0.0.1
port = 8080
```

Alternatively you can also create a RelStorage setup like this:

```sh
./venv/bin/pdm run create_relstorage_instance
```

Adjust the data base connection string in `instance1/etc/zope.conf`:

```
<zodb_db main>
    <relstorage>
      blob-dir $INSTANCE/var/blobstorage
      <postgresql>
        dsn dbname='my_plone_database' user='my_plone_db_user' host='localhost' password='SECRET'
      </postgresql>
    </relstorage>
   mount-point /
</zodb_db>
```

Start the instance

```sh
./venv/bin/pdm run start_instance
```

In production you want to add this to something like supervisor or a systemd script.
