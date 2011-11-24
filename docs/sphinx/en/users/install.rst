.. Installation
  #-----------------------------------------------------------------------------
  # $Id$
  #
  # Project: EOxServer <http://eoxserver.org>
  # Authors: Stephan Krause <stephan.krause@eox.at>
  #          Stephan Meissl <stephan.meissl@eox.at>
  #
  #-----------------------------------------------------------------------------
  # Copyright (C) 2011 EOX IT Services GmbH
  #
  # Permission is hereby granted, free of charge, to any person obtaining a copy
  # of this software and associated documentation files (the "Software"), to
  # deal in the Software without restriction, including without limitation the
  # rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
  # sell copies of the Software, and to permit persons to whom the Software is
  # furnished to do so, subject to the following conditions:
  #
  # The above copyright notice and this permission notice shall be included in
  # all copies of this Software or works derived from this Software.
  #
  # THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
  # IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
  # FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
  # AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
  # LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING 
  # FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
  # IN THE SOFTWARE.
  #-----------------------------------------------------------------------------

.. index::
    single: EOxServer Installation
    single: Installation

.. _Installation:

Installation
============

Before you can use EOxServer, you’ll need to get it installed. Following this 
guide will give you a simple, minimal though working installation.

.. index::
    single: EOxServer Dependencies
    single: Dependencies

Dependencies
------------

EOxServer depends on some external software. Table: 
":ref:`table_eoxserver_dependencies`" below shows the minimal required software 
to run EOxServer.

.. _table_eoxserver_dependencies:
.. table:: EOxServer Dependencies

    +-----------+------------------+-------------------------------------------+
    | Software  | Required Version | Description                               |
    +===========+==================+===========================================+
    | Python    | >= 2.5, < 3.0    | Scripting language                        |
    +-----------+------------------+-------------------------------------------+
    | Django    | >= 1.2           | Web development framework written in      |
    |           |                  | Python including the GeoDjango extension  |
    |           |                  | for geospatial database back-ends.        |
    +-----------+------------------+-------------------------------------------+
    | GDAL      | >= 1.8.0         | Geospatial Data Abstraction Library       |
    |           | (for rasdaman    | providing common interfaces for accessing |
    |           | support)         | various kinds of raster and vector data   |
    |           |                  | formats and including a Python binding    |
    |           |                  | which is used by EOxServer                |
    +-----------+------------------+-------------------------------------------+
    | MapServer | >= 6.0           | Server software implementing various OGC  |
    |           |                  | Web Service interfaces including WCS and  |
    |           |                  | WMS. Includes a Python binding which is   |
    |           |                  | used by EOxServer.                        |
    +-----------+------------------+-------------------------------------------+

EOxServer is written in `Python <http://www.python.org/>`_ and uses the 
`Django <https://www.djangoproject.com>`_ framework which requires a 
Python version from 2.4 to 2.7. Due to backwards incompatibilities in Python 
3.0, Django and thus EOxServer does not currently work with Python 3.0.

EOxServer makes heavy usage of the `OSGeo <http://osgeo.org>`_ projects 
`GDAL <http://www.gdal.org>`_ and `MapServer <http://mapserver.org>`_.

EOxServer additionally needs a database but since Python 2.5 or later 
is required which includes a lightweight database called 
`SQLite <http://sqlite.org/>`_, you can skip this step for now. However, if 
you'd like to work with a "large" database engine in an operational environment 
we recommend installing `PostgreSQL <http://www.postgresql.org/>`_ 
and its extension `PostGIS <http://postgis.refractions.net/>`_ (see 
`Django database notes <https://docs.djangoproject.com/en/1.3/ref/databases/>`_ 
and `GeoDjango installation 
<https://docs.djangoproject.com/en/1.3/ref/contrib/gis/install/>`_).

Installing EOxServer
--------------------

There are several easy options to install EOxServer:

* Install an official release of EOxServer, the best approach for users who 
  want a stable version and aren't concerned about running a slightly older 
  version of EOxServer. You can install EOxServer either from 
  
  * `PyPI - the Python Package Index <http://pypi.python.org/pypi>`_ using 
    `pip <http://www.pip-installer.org/en/latest/index.html>`_:
    ::
    
      sudo pip install eoxserver
    
  * Or from the `EOxServer download page <http://eoxserver.org/wiki/Download>`_ 
    using pip:
    ::
    
      sudo pip install http://eoxserver.org/export/head/downloads/EOxServer-<version>.tar.gz
    
    or manual:
    ::
    
      wget http://eoxserver.org/export/head/downloads/EOxServer-<version>.tar.gz .
      tar xvfz EOxServer-<version>.tar.gz
      cd EOxServer-<version>
      sudo python setup.py install

* Install the latest development version, the best option for users who 
  want the latest-and-greatest features and aren't afraid of running 
  brand-new code. Make sure you have `Subversion 
  <http://subversion.tigris.org/>`_ installed and install EOxServer's 
  main development branch (the trunk) using pip:
  ::
  
    sudo pip install svn+http://eoxserver.org/svn/trunk
    
  or manual:
  ::
  
    svn co http://eoxserver.org/svn/trunk/ eoxserver-trunk
    cd eoxserver-trunk
    sudo python setup.py install

To upgrade an existing installation of EOxServer simply add the `--upgrade` 
switch to your pip command e.g.:
::

  sudo pip install --upgrade eoxserver


If the directory EOxServer is installed to is not on the Python path, you will 
have to configure the deployed instances accordingly, see 
:ref:`EOxServer Deployment` below.

.. index::
    single: EOxServer Instance Creation
    single: Instance Creation

Creating an Instance
--------------------

Speaking of EOxServer we distinguish the EOxServer distribution (the code that 
implements the different services the software provides) and EOxServer 
instances (a collection of data and configuration files that enables deployment 
of the software.

We recommend to use the :file:`eoxserver-admin.py` script that comes with 
EOxServer. It provides the command `create_instance` in order to create an
EOxServer instance:

    Usage: ``eoxserver-admin.py create_instance [options] INSTANCE_ID``
    
    Create a new EOxServer instance ``INSTANCE_ID`` in the root directory with 
    name ``INSTANCE_ID`` in the given (optional) directory. If the 
    ``--init_spatialite`` flag is set, then an initial sqlite database will be 
    created and initialized.
    
    Options:
    
    -h, --help           show help message and exit
    -d DIR, --dir=DIR    Optional base directory. Defaults to the current 
                         directory.
    --initial_data=DIR   Location of the initial data. Must be JSON.
    --init_spatialite    Flag to initialize the sqlite database.
    --mapscript-dir=DIR  Optional path to the MapServer mapscript library. Set 
                         this to use a customized mapscript version instead of 
                         the one on the regular Python path.

.. index::
    single: EOxServer Configuration
    single: Configuration

Configuration
~~~~~~~~~~~~~

Every EOxServer instance has three configuration files:

* ``settings.py`` - `template 
  <http://eoxserver.org/browser/trunk/eoxserver/conf/TEMPLATE_settings.py>`__
* ``conf/eoxserver.conf`` - `template 
  <http://eoxserver.org/browser/trunk/eoxserver/conf/TEMPLATE_eoxserver.conf>`__
* ``conf/template.map`` - `template 
  <http://eoxserver.org/browser/trunk/eoxserver/conf/TEMPLATE_template.map>`__

For each of them there is a template in the ``eoxserver/conf`` directory of the 
EOxServer distribution (referenced above) which is copied and adjusted by the 
`create_instance` command of the :file:`eoxserver-admin.py` script to the 
instance directory. If you create an EOxServer instance without the script you 
can copy those files and edit them yourself.

The file ``settings.py`` contains the Django configuration. Settings that need 
to be customized:

* ``PROJECT_DIR``: Absolute path to the instance directory.
* ``MAPSCRIPT_PATH``: (Optional) Absolute path to the Python MapScript build 
  directory. Set this if you want to use a custom MapScript version (e.g. 
  generated from the MapServer trunk).
* ``DATABASES``: The database connection details. If using SQLite (the default) 
  be sure to use the absolute path to the ``config.sqlite`` file (see below).

You can also customize further settings, for a complete reference please refer 
to the `Django settings overview 
<https://docs.djangoproject.com/en/1.3/topics/settings/>`_.

The file ``conf/eoxserver.conf`` contains EOxServer specific settings. Please 
refer to the inline documentation for details.

The file ``conf/template.map`` contains basic metadata for the OGC Web Services 
used by MapServer. For more information on metadata supported please refer to 
the `MapServer Mapfile documentation 
<http://mapserver.org/mapfile/index.html>`_.

Once you have created an instance, you have to configure and synchronize the 
database. If using the `create_instance` command of the 
:file:`eoxserver-admin.py` script with the ``--init_spatialite`` flag, all you 
have to do is:

* Make sure EOxServer is on your ``PYTHONPATH`` environment variable
* run ``python manage.py syncdb`` in your instance directory

Using a PostgreSQL/PostGIS database back-end configuration is a little bit more 
complex. Please refer to `GeoDjango Database API 
<https://docs.djangoproject.com/en/1.3/ref/contrib/gis/db-api/>`_ for more 
instructions.

.. TODO: Logfile handling:
    configuration in settings.py and eoxserver.conf
    logrotate, etc.

.. index::
    single: EOxServer Deployment
    single: Deployment

.. _EOxServer Deployment:

Deployment
~~~~~~~~~~

EOxServer is deployed using the Python WSGI interface standard as any other 
`Django application <https://docs.djangoproject.com/en/1.3/howto/deployment/>`_.
The WSGI endpoint accepts HTTP requests passed from the web server and 
processes them synchronously. Each request is executed independently.

In the following we present the way to deploy it using the `Apache2 Web Server 
<http://httpd.apache.org>`_ and its `mod_wsgi 
<http://code.google.com/p/modwsgi/>`_ extension module.

The deployment procedure consists of the following:

* create a ``deployment`` subdirectory in your instance
* copy ``TEMPLATE_wsgi.py`` from the EOxServer distribution ``eoxserver/conf``
  directory there under the name ``wsgi.py``
* Customize ``wsgi.py``
* Customize the Apache2 configuration file
* Restart the Web Server

In ``wsgi.py``, two items need to be customized. First, the Python path has to 
be set properly and second, the Django settings module (``settings.py``) has to 
be configured. The places where to fill in the right names are indicated in the 
file.

In the Apache2 configuration file for your server, e.g. 
``/etc/apache2/sites-enabled/000-default``, please add the following lines:
::

    Alias /<url> <absolute path to instance dir>/deployment/wsgi.py
    <Directory "<absolute path to instance dir>/deployment"> 
            AllowOverride None 
            Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch 
            AddHandler wsgi-script .py 
            Order Allow, Deny
            Allow from all 
    </Directory>

This setup will deploy your instance under the URL ``<url>`` and make it 
publicly accessible.

Data Registration
~~~~~~~~~~~~~~~~~

To insert data into an EOxServer instance there are several ways. One is the
admin interface, which is explained in detail in the `Administration Web
Application Tutorial`_.

Another convenient way to register datasets is the command line interface to
EOxServer. As a Django application, the instance can be configured using the
`manage.py <https://docs.djangoproject.com/en/dev/ref/django-admin/>`_ script.

EOxServer provides a specific command to insert datasets into the instance,
called ``eoxs_register_dataset``. It is invoked from command line from your
instance base folder:
::

    python manage.py eoxs_register_dataset --data-file DATAFILES --rangetype RANGETYPE

The mandatory parameter `data-file` is a list of at least one path to a file
containing the raster data for the datasets to be inserted. The files can be
in any compliant (GDAL readable) format.

For each data file there has to be one meta-data file containing earth
observation specific meta-data. The optional parameter ``--metadata-file`` shall
contain a list of paths to these files, where the items of this list refer to
the data files with the same index of the according option. This parameter can
also be omitted, in this case for each data file a metadata file is asumpted
with the same path, but with an `.xml` extension. When inserting rasdaman
datasets, this parameter is manadatory, since the meta-data cannot be retrieved
from within the rasdaman database.

For each dataset a coverage ID can be specified with the ``--coverage-id``
parameter. As with the ``--metadata-file`` option, the items of the list refer
to the items of the ``--data-file`` list. If omitted, an ID is generated using
the data file name.

Also mandatory is the parameter ``--rangetype``, the name of a range type which
has to be already present in the instances database.

The parameters ``--dataset-series`` and ``--stitched-mosaic`` allow the dataset
to be inserted into all dataset series and rectified stitched mosaics specified
by their EO IDs.

The ``--mode`` parameter specifies the location of the data and metadata files.
This can either be `local`, `ftp` or `rasdaman`, whereas the default is `local`.

This is an example usage of the ``eoxs_register_dataset`` command:
::

    python manage.py eoxs_register_dataset --data-file data/meris/mosaic_MER_FRS_1P_RGB_reduced/*.tif --rangetype RGB --dataset-series MER_FRS_1P_reduced --stitched-mosaic mosaic_MER_FRS_1P_RGB_reduced -v3

In this example, the parameter ``--metadata-file`` is omitted, since these files
are in the same location as the data files and only differ in their extension.
Also note that the ``--data-file`` parameter uses a shell wildcard `*.tif` which
expands to all files with the `.tif` extension in that directory. This
funcitonality is not provided by EOxServer but by the operating system or the
executing shell and is most certainly platform dependant.

The registered dataset is now inserted to each one dataset series and rectified
stitched mosaic.
