Flight Following
================

Data Flow
---------
 - Telemetry comes in via AMQ:
   + from DGS1 -> ECH-DGETS 
     - AMQ Topic=
   + from DGS2 -> WCH-DGETS 
     - AMQ Topic=

.. note::

   gr-syersFF service handles processing the telemetry.
   gr-syersFF calls /opt/GRamp/scripts/SyersAuxParser.py


SyersAuxParser listens on AMQ, unpacks the 2400 byte SCU cstruct packet
and writes the values to shared memory as dict {$thread:[info]}

- There are three wsgi scripts that read from this shared memory:

   - /opt/GRamp/apache2/wsgi/SYERS/AuxStatus.wsgi
     - returns xml with current ops (that have aux data)
     - called by /opt/GRamp/apache2/htdocs/img_svr/ff/js/grids/flight_following.js
     - xml can be seen in browser at dgets/syersffstatus?thread=S6V1

   - /opt/GRamp/apache2/wsgi/SYERS/SyersTelemetry.wsgi
     - used for ff without tracking
     - returns xml with telemetry info
     - called by /opt/GRamp/apache2/htdocs/img_svr/ff/php/LaunchKML.php
     - xml can be seen in browser at dgets/syerstelem?thread=S6V1

   - /opt/GRamp/apache2/wsgi/SYERS/SyersTelemetryUpdate.wsgi
     - used for ff with tracking on
     - called every 2 seconds with updated xml of telemetry
     - called by /opt/GRamp/apache2/htdocs/img_svr/ff/php/LaunchKML.php
     - xml can be seen in browser at dgets/syerstelemupdate?thread=S6V1


Logs for FF are located:
 - /raid/DGETS_DATA/dgets_logs/SyersAux.log
 - Only errors are logged

Restart FF service via

  ``svcadm restart gr-syersFF``

   or via the admin_hud
