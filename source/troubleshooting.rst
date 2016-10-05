Troubleshooting
===============

Database Problems
-----------------

.. note::

   phpmyadmin is an easy to use wui to manage/maintain DGETS mysql db
   http://dgets-east.480iw.langley.af.smil.mil/phpmyadmin/index.php
   login: root
   pw: R@isin1

- If mission ID query is not returning any results:
  #. Log into phpmyadmin
  #. Select the Imagery Database
  #. Select the `tgt` table
  #. Select the browse tab
  #. Sort by target id, ensure the newest entry is on top
  #. Look for any entries that have bad timestamps
    ie: "0000-00-00" vs "2010-04-07"
  #. Delete bad row  

Unable to Federate to DGETS
---------------------------

- If unable to federate data to DGETS even after restarted local GRDM (and DGETS watcher not updating)
  on local is10 server
  
  .. code:: console
     
     clrs status | grep a-is10-services
     clrs disable a-is10-services-grdm-rs
     cd /opt/p3i/gnd/GRgmm/FederationStorage
     rm missionID.xml
     clrs enable a-is10-services-grdm-rs

Images not viewable
-------------------

- If data stops processing or not viewable after selecting an image in Image query
  #. ``cd /raid/DGETS_DATA/PROCESSING``
  #. ``mv * ../INPUT``


Admin Hud
---------

Admin Hud unresponsive - check dns status (try entering DGETS IP manually)
May have to reset Apache
