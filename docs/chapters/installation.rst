.. 
   ****************************************************************************
    pgRouting Manual
    Copyright(c) pgRouting Contributors

    This documentation is licensed under a Creative Commons Attribution-Share  
    Alike 3.0 License: http://creativecommons.org/licenses/by-sa/3.0/
   ****************************************************************************

.. _installation:

Installation and Requirements
===============================================================================

For this workshop you need:

* Preferable a Linux operating system like Ubuntu
* An editor like Gedit or Mousepad
* `Geoserver <http://geoserver.org>`_ for the routing application
* Internet connection

All required tools are available on the `OSGeo LiveDVD <http://live.osgeo.org>`_, so the following reference is a quick summary of how to install it on your own computer running Ubuntu 12.04 or later.


pgRouting
-------------------------------------------------------------------------------

pgRouting on Ubuntu can be done using packages from a `Launchpad repository <https://launchpad.net/~georepublic/+archive/pgrouting-unstable>`_: 

All you need to do is to open a terminal window and run:

.. code-block:: bash
	
	# Add pgRouting launchpad repository
	sudo add-apt-repository ppa:georepublic/pgrouting-unstable
	sudo apt-get update

	# Install pgRouting package
	sudo apt-get install postgresql-9.1-pgrouting 

	# Install osm2pgrouting package
	sudo apt-get install osm2pgrouting

	# Install workshop material (optional)
	sudo apt-get install pgrouting-workshop

	# For FOSS4G 2013 workshop:
	# Download and install from http://trac.osgeo.org/osgeo/wiki/Live_GIS_Workshop_Install
	wget --no-check-certificate https://launchpad.net/~georepublic/+archive/pgrouting-unstable/+files/pgrouting-workshop_[version]_all.deb
	sudo dpkg -i pgrouting-workshop_[version]_all.deb

This will also install all required packages such as PostgreSQL and PostGIS if not installed yet.

.. note::

	* Once pgRouting 2.0 has been released it will be available in the ``stable`` repository on Launchpad.
	* To be up-to-date with changes and improvements you might run ``sudo apt-get update & sudo apt-get upgrade`` from time to time, especially if you use an older version of the LiveDVD.
	* To avoid permission denied errors for local users you can set connection method to ``trust`` in ``/etc/postgresql/9.1/main/pg_hba.conf`` and restart PostgreSQL server with ``sudo service postgresql restart``.

	.. code::

		local   all             postgres                                trust
		local   all             all                                     trust
		host    all             all             127.0.0.1/32            trust
		host    all             all             ::1/128                 trust

	``pg_hba.conf`` can be only edited with "superuser" rights, ie. from the terminal window with 

	.. code::

		sudo nano /etc/postgresql/9.1/main/pg_hba.conf

	To close the editor again hit ``CTRL-X``.


Workshop
-------------------------------------------------------------------------------

When you installed the workshop package you will find all documents in ``/usr/share/pgrouting/workshop/``.

We recommend to copy the files to your home directory and make a symbolic link to your webserver's root folder:

.. code-block:: bash
	
	cp -R /usr/share/pgrouting/workshop ~/Desktop/pgrouting-workshop
	sudo ln -s ~/Desktop/pgrouting-workshop /var/www/pgrouting-workshop

You can then find all workshop files in the ``pgrouting-workshop`` folder and access to

* Web directory: http://localhost/pgrouting-workshop/web/
* Online manual: http://localhost/pgrouting-workshop/docs/html/

.. note::

	Additional sample data is available in the workshop ``data`` directory. To extract the file run ``tar -xzf ~/Desktop/pgrouting-workshop/data.tar.gz``.


.. _installation_load_functions:

Add pgRouting Functions to database
-------------------------------------------------------------------------------

Since **version 2.0** pgRouting functions can be easily installed as extension. This requires:

* PostgreSQL 9.1 or higher
* PostGIS 2.x installed as extension

If these requirements are met, then open a terminal window and execute the following commands (or run these commands in pgAdmin 3:

.. code-block:: bash

	# login as user "postgres" 
	psql -U postgres

	# create routing database
	CREATE DATABASE routing;
	\c routing

	# add PostGIS functions 
	CREATE EXTENSION postgis;

	# add pgRouting core functions
	CREATE EXTENSION pgrouting;
	

.. note::

	If you're looking for the SQL files containing pgRouting function, you can find them in ``/usr/share/postgresql/9.1/contrib/pgrouting-2.0/``:

	.. code-block:: bash

		-rw-r--r-- 1 root root  4126 Jun 18 22:30 pgrouting_dd_legacy.sql
		-rw-r--r-- 1 root root 43642 Jun 18 22:30 pgrouting_legacy.sql
		-rw-r--r-- 1 root root 40152 Jun 18 22:30 pgrouting.sql

Data
-------------------------------------------------------------------------------

The pgRouting workshop will make use of OpenStreetMap data, which is already available on the LiveDVD. If you don't use the LiveDVD or want to download the latest data or the data of your choice, you can make use of OpenStreetMap's API from your terminal window:

.. code-block:: bash
	
	# Download using Overpass XAPI (larger extracts possible than with default OSM API)
	BBOX="-1.2,52.93,-1.1,52.985"
	wget --progress=dot:mega -O "sampledata.osm" "http://www.overpass-api.de/api/xapi?*[bbox=${BBOX}][@meta]"

More information how to get OSM data:

	* OpenStreetMap download information in http://wiki.openstreetmap.org/wiki/Downloading_data
	* OpenStreetMap data is available at the LiveDVD in ``/usr/local/share/osm/``

An alternative for very large areas is the download services of `Geofabrik <http://download.geofabrik.de>`_. 
Download a country extract and unpack the data like this:

.. code-block:: bash

	wget --progress=dot:mega http://download.geofabrik.de/[path/to/file].osm.bz2
	bunzip2 [file].osm.bz2
	
.. warning::

	Data of a whole country might be too big for the LiveDVD as well as processing time might take very long.  
	






