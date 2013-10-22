MusicBrainz Database Tools
==========================

SQLAlchemy Models
-----------------

If you are developing a Python application that needs access to the MusicBrainz
data, you can use the `mbdata.models` module to get [SQLAlchemy](http://www.sqlalchemy.org/)
models mapped to the MusicBrainz database tables.

All tables from the MusicBrainz database are mapped, all foreign keys have one-way
relationships set up and some models, where it's essential to access their
related models, have two-way relationships (collections) set up.

In order to work with the relationships efficiently, you should use the appropriate
kind of [eager loading](http://docs.sqlalchemy.org/en/rel_0_8/orm/loading.html).

Example:

    #!python
    >>> from sqlalchemy import create_engine
    >>> from sqlalchemy.orm import sessionmaker
    >>> from mbdata.models import Artist
    >>> engine = create_engine('postgresql://musicbrainz:musicbrainz@127.0.0.1/musicbrainz', echo=True)
    >>> Session = sessionmaker(bind=engine)
    >>> session = Session()
    >>> artist = session.query(Artist).filter_by(gid='b10bbbfc-cf9e-42e0-be17-e2c3e1d2600d').first()
    >>> print artist.name

HTTP API
--------

**Note:** This is very much a work in progress. It is not ready to use yet. Any help is welcome.

There is also a HTTP API, which you can use to access the MusicBrainz data using
JSON or XML formats over HTTP. This is useful if you want to abstract away the
MusicBrainz PostgreSQL database.

Installation:

    #!sh
    virtualenv --system-site-packages e
    . e/bin/activate
    pip install -r requirements.txt
    python setup.py develop

Configuration:

    #!sh
	cp settings.py.sample settings.py
	vim settings.py

Start the development server:

    #!sh
    MBDATA_API_SETTINGS=`pwd`/settings.py python -m mbdata.api.app

Query the API:

    #!sh
    curl 'http://127.0.0.1:5000/v1/artist/details?id=b10bbbfc-cf9e-42e0-be17-e2c3e1d2600d'

For production use, you should use server software like
[uWSGI](http://projects.unbit.it/uwsgi/) and
[nginx](http://nginx.org/) to run the service.

Web Application
---------------

To help you view the contents of the MusicBrainz database, there is also a simple JavaScript
application that uses the HTTP API and allows you to browse database.

Start the development server:

    #!sh
    MBDATA_API_SETTINGS=`pwd`/settings.py python -m mbdata.web.app
