Admin Hud
=============

.. note::

   Admin Hud is used to remotely maintain typical sysad funtions via a web gui
   Admin Hud can be reached at: http://dgets_url/img_svr/server
   default user:pass = integ/#R1

User Interface
---------------
There are 7 widgets to help aid in SA and Maintenance.

Server Processes
++++++++++++++++

Server processes shows the status (or PID) for the main processes for DGETS. There is an Info icon that will
explain what the process does and a Reset icon to perform a reset for the process/pid

- ActiveMQ
  - Reset if:
    - Flight Following stops
    - Mission Plans are not showing up
  - Action:
    - Resets gr-activemq service

- Apache
  - Reset if:
    - DGETS webpage not working
  - Action:
    - Resets gr-apache2 service

- Flight Following
  - Reset if:
    - Flight Following stops
    - Mission Plans are not showing up
  - Action:
    - Resets gr-dgetsFF service

- KDU Server
  - Reset if:
    - JPIP server not working
  - Action:
    - Stop/Start /etc/rc3.d/S99

- MYSQL
  - Reset if:
    - Chat timestamps stop updating
  - Action:
    - Resets gr-mysql service

- ProcessImages Main (PID of daemon owning 4 PI subs)
  - Reset if:
    - Files are not moving out of ppi
  - Action:
    - Stop/Start /etc/rc3.d/S98P3I_PROC
    - Moves images from processing back to ppi

- ProcessImages Sub (4 PIDS of PI's kicked off by PI Main) 
  - Reset if:
    - Files are not moving out of ppi
  - Action:
    - Stop/Start /etc/rc3.d/S98P3I_PROC
    - Moves images from processing back to ppi

- Tile NITF Instances
  - Reset if:
    - Files are not moving out of DGETS_DATA/INPUT
  - Action:
    - Stop/Start /etc/rc3.d/S98P3I_PROC
    - Moves images from PROCESSING back to DGETS_DATA/INPUT

- Tile Web Instances
  - Reset if:
    - Files are not moving out of TILE
  - Action:
    - Stop/Start /etc/rc3.d/S98P3I_PROC
    - Moves images from PROCESSING back to TILE

Flight Following
++++++++++++++++

Shows SA for current missions (that have telem)

ProcessImages Files
+++++++++++++++++++

Shows files currently being processed.

Widget shows filename, age, reprocessing icon, trash icon

ProcessImages Save Dir
++++++++++++++++++++++

Shows data that has been processed.

Widget show MissionID, number of images, trash icon

Disk Usage
++++++++++

Shows File System statistics for:
 - /raid
 - /raid/DGETS_DATA (SYERS)
 - /raid/mission_images (GH/ASARS)
 - Global / (ensure audits don't fill up /var)


Missions Delete Status
++++++++++++++++++++++

Shows status of PI save dir cleanup

DGETS MX
++++++++

Three tabs to clean
 - Imagery
   - Can delete entire missions' imagery with delete icon
   - Can delete individual scenes by clicking ? icon

 - Plans
   - Delete icon will delete SP from DB.

 - DB Restore
   - Option to restore DB to earlier state (goes back 1 week)

Processes
---------

Admin hud proceses contolled by /etc/rc3.d/S99SYERS
 - statusd.py
   - controls showing pids, status, etc
 - resetd.py
   - controls the resets via admin hud



Resets
++++++

To reset Admin Hud procs:

``/etc/rc3.d/S99SYERS stop/start``

Logs for statusd.py and resetd.py are located:
 - /raid/DGETS_DATA/dgets_logs/statusd.log
 - /raid/DGETS_DATA/dgets_logs/resetd.log

