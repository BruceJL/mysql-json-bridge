mysql-json-bridge
=================
This is a fork of the mysql-json-bridge as found at:
http://github.com/rackerhacker/mysql-json-bridge. It has been changed such that
it acts as a restful database between an Ember.js client and a MariaDB (nee
MySQL database.)

It has been updated from the source to Python 3 and to support the default
Ember.js RESTadapter.

Installation & Startup
----------------------
Install a few prerequisites:

   The Debian packages required to run this software are as follows:
   python3
   python3-flask
   python3-pretty-yaml
   python3-jsonpickle
   python3-pymysql
   python3-inflect
   python3-dateutil
   python3-flash-cors

Get the source:

    git clone http://github.com/BruceJL/mysql-json-bridge
    cd mysql-json-bridge
    python app.py

Configuration
-------------
Make a conf.d directory with separate database configuration files:

    # conf.d/database1.yaml
    ---
    identifier: 'prod.database1'
    scheme: 'mysql'
    username: 'database1'
    password: 'secret_password'
    database: 'database1'
    hostname: 'database1.domain.com'
    enabled: 'True'

    # conf.d/database2.yaml
    ---
    identifier: 'staging.database2'
    scheme: 'mysql'
    username: 'database2'
    password: 'secret_password'
    database: 'database2'
    hostname: 'database2.domain.com'
    enabled: 'True'

Usage
-----
To issue a query to the bridge, simply make an HTTP POST to the appropriate URL.
Your URL should be something like this:

    http://localhost:5000/<database>/<table>

Will pull all entries for that table.

Example wsgi file for usage with a web server is supplied as wsgi.py. It seems
to run will using gunicorn.

*IMPORTANT* security considerations
-----------------------------------
**The base mysql-json-bridge server doesn't do any query filtering nor does it
do any authentication.  You'd need to configure that yourself within your web
server.**

Also, be very careful with the user you configure in your `environments.yml`.
If the user has write access to your database, people could issue UPDATE and
DELETE statements through the bridge.

If you create read-only MySQL users for the bridge to use, **ensure that those
users have read access *only* to the databases that you specify.**  Giving
global read access to a user allows them to read your `mysql.user` table which
contains hashed passwords.  *This could lead to a very bad experience.*

Got improvements?  Found a bug?
-------------------------------
Issue a pull request or open an issue in GitHub.
I appreciate and welcome all feedback you have!

Tip of the hat
--------------------
Big tip of the hat to major for the material to make the fork.
