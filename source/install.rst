Installation
============

SAMP.run
--------

Self installer script for Apache,MySQL,PHP. Installs into /opt/Samp

``# ./SAMP<DATE>.run --target /opt/Samp``

dgets_cfg - Put into place post Samp.run
----------------------------------------

.. code:: console

    httpd.conf:/opt/Samp/apache2/conf
    httpd-mpm.conf:/opt/Samp/apache2/conf/extra
    my.cnf:/opt/Samp/mysql
    php.ini:/opt/Samp/php5/etc

MySQL setup
-----------

.. code:: console

    # groupadd mysql
    # useradd -g mysql mysql
    # cd /opt/Samp/mysql
    # chown -R mysql:mysql .
    # scripts/mysql_install_db --user=mysql
    # chown -R root .
    # chown -R mysql data
    # chmod 777 /opt/Samp/mysql


Apache setup
--------------

.. code:: console

    # groupadd -g 80 webservd
    # useradd -u 80 -g webservd webservd


**Make Link**
 ``# ln -s /opt/Samp /opt/amp``

LOCAL.run
----------

Self installer script for local utilities. Installs into /usr/local

.. code:: console

    # ./LOCAL<DATE>.run --target /usr/local
    # cp /usr/local/bin/perl /usr/bin

DGETS_SRC
---------
Includes ext3/4, phpmyadmin, external javascript, icon images and DGETS src. Installs into /opt/Samp/apache/htdocs. 

.. code:: console

    # cd /opt/Samp/apache2
    # unzip htdocs.zip
    # cd /opt/Samp/apache2/htdocs
    # unzip img_svr.zip
    # cd img_svr
    # ln -s ../ext-3* ext; ln -s ../ext-4* ext4
    # cd MSUM
    # ln -s ../ext .; ln -s ext/examples/ux .


IP Scripts Config
-----------------
- Change SERVER_NAME, ORIG_SITE:
    img_svr/scripts/TILE_NITF.sh
- Change IP address:
    img_svr/scripts/lib/kmlTemplates.py
- Change STOMP_HOST:
    img_svr/config/CommonSetup.ini
- Change IP address:
    img_svr/RESULTS/JPIP_LAUNCHER.php
- Change ServerName:
    /opt/Samp/apache2/conf/httpd.conf

DB Conf
---------
img_svr/config/conf.php

Startup Scripts
----------------

.. code:: console

    # cp init.d/S99dgets /etc/rc3.d/S99dgets
    # /etc/rc3.d/S99/dgets start/stop

DGETS Startup services
----------------------

.. code:: console

    # svccfg import /opt/Samp/lib/svc/manifest/dgets-apache2.xml
    # svccfg import /opt/Samp/lib/svc/manifest/dgets-mysql.xml
    # svccfg import /opt/Samp/lib/svc/manifest/dgets-amq.xml
    # svcadm enable dgets-apache2; svcadm enable dgets-mysql; svcadm enable dgets-amq

dgets_sql
---------

.. note::
    Files needed to be inserted int MySQL database. I use utas/p3i for username/password for user access.

.. code:: console

    # cd /opt/Samp/mysql; bin/mysql_secure_installation # Add root user password (Can be anything, just don't forget) and select Yes for all rest
    # /opt/Samp/mysql/bin/mysql -uroot -p < chat.sql
    # /opt/Samp/mysql/bin/mysql -uroot -p < dgets_db.sql
    # /opt/Samp/mysql/bin/mysql -uroot -p < sensor_status.sql

.. code:: console

    # /opt/Samp/mysql/bin/mysql -uroot -p
    (Copy and Paste below)

    CREATE USER 'utas'@'localhost' IDENTIFIED BY 'p3i';
    GRANT SELECT, INSERT, UPDATE, DELETE ON `chat`.* TO 'utas'@'localhost';
    GRANT SELECT, INSERT, UPDATE, DELETE ON `dgets\_db`.* TO 'utas'@'localhost';
    GRANT SELECT, INSERT, UPDATE, DELETE ON `sensor\_status`.* TO 'utas'@'localhost';
    FLUSH PRIVILEGES;



ADDITIONAL Notes:
+++++++++++++++++

- Access Homepage: http://xxx.xxx.xxx.xxx/img_svr/desktop_app

- Can add /usr/local/lib:/usr/local/ssl/lib:/opt/Samp/mysql/lib to LD_LIB path via LD_LIBRARY_PATH 

- Swapping between UNCLASS and SECRET banners

.. code:: console

    # img_svr/scripts/change_banner.sh [SECRET/UNCLASS]
