Basic Data Flow
===============

.. note::
    There are two main DGETS servers (ECH & WCH). The data will come into DGS-1 and DGS-2 
    and then be disseminated near-realtime to ECH (from DGS-1) and WCH (from DGS-2)

Web Tiles, Target Query jpegs, Full Resolution NITFs will be available shortly after
reaching the DGETS servers (time of availability dependent on processing, mission load, etc.)

Full Resolution NITFs
---------------------

Full Resolution NITFs are processed on DGETS via x86 ProcessImages. MSPs are disseminated from
DGS-1 -> ECH-DGETS & DGS-2 -> WCH-DGETS via GRDM/RDTP from is10 servers. After MSP.ntf is received
at DGETS, a typical data flow to the DGS occurs:

  - CRS listens, receives and logs received files
  - MIN renames NITF to formart MvSyersFiles.pl can handle
  - MvSyersFiles.pl moves imagery into PI input directory
  - PI ingests MSP.ntf and outputs desired products

Full Resolution Directory Paths:
++++++++++++++++++++++++++++++++

  .. note::
     
     Processes that control receiving and processing data are kicked off in
     /etc/rc3.d/S95syers & /etc/rc3.d/S83P3I_PROC


  * FR NITFS are received from via rdtp:

    - log path: 

      + /raid/DGETS_DATA/dgets_logs/crs_dgs1.log
      + /raid/DGETS_DATA/dgets_logs/crs_dgs2.log

    - input destination:

      + /raid/DGETS_DATA/mission_images/fr

  * MIN moves files from F_234234 name to MISSION_SCENE_TIME.ntf

    - log path:

      + /raid/DGETS_DATA/dgets_logs/min.log

    - input path:

      + /raid/DGETS_DATA/mission_images/fr

    - output path:

      + /raid/DGETS_DATA/mission_images/fr (new name)

  * MvSyersFiles (formerly ELT) changes mMISSION_SCENE_TIME.ntf
    To our S2 format and drops file in ProcessImages input directory

    - log path:

      + /raid/DGETS_DATA/dgets_logs/nitf_rename.out
    - input path:

      + /raid/DGETS_DATA/mission_images/fr
    - output path:

      + /raid/DGETS_DATA/PI (new name)

  * PI picks up data and creates False color composites in following dirs:

    - log path:

      + /raid/DGETS_DATA/dgets_logs/procImages1.out
      + /raid/DGETS_DATA/dgets_logs/procImages2.out
      + /raid/DGETS_DATA/dgets_logs/procImages3.out
      + /raid/DGETS_DATA/dgets_logs/procImages4.out
      + /raid/DGETS_DATA/dgets_logs/procImages5.out

    - input path:

      + /raid/DGETS_DATA/PI

    - working path:

      + /raid/DGETS_DATA/PROCESSING

    - output path:

      + /raid/DGETS_DATA/MISSIONS


Web Tiles
---------

.. note::
   
   Web tile processing is handled by GDAL. This processing handles 50 tiles at
   a time (and can get backed up depending on mission load)

   Tiles created by gdal are PNGs that are overlayed onto Google Earth. These
   are currently set to overlay the IRC.ntf for each scene

   Processing handled by S98P3I_PROC which calls:
     /opt/GRamp/apache2/htdocs/img_svr/scripts/Main.sh
      /opt/GRamp/apache2/htdocs/img_svr/scripts/TILE_NITF.sh (GE_OVERLAY function)
       /opt/GRamp/apache2/htdocs/img_svr/scripts/libGOOGLE_OV.sh (GE_OVERLAY function)


GE Tile Directories
++++++++++++++++++++
    
    - Input dir:

      + /raid/DGETS_DATA/INPUT
   
    - Processing dir:

      + /raid/DGETS_DATA/PROCESSING

    - Output dir: ???

      + /raid/DGETS_DATA/DONE

    - PNG dir:
    
      + /raid/DGETS_DATA/MISSIONS/MISSIONID/SCENE.dir/GE_FILES

.. information::

   what is HIGH.png and LOW.png?
   what is 14,15,16,17,18 in /raid/DGETS_DATA/MISSIONS/MISSIONID/SCENE.dir/GE_FILES

Target Query 
---------------

.. note::
   
   Target Query tiles are produced by GDAL. This processing handles 50 tiles at
   a time (and can get backed up depending on mission load)

   Tiles created by gdal are JPGs that are used in the Target Query web gui. There are folders
   for each level of zoom and an overview jpg used for thumbnail previews

   Processing handled by S98P3I_PROC which calls:
     /opt/GRamp/apache2/htdocs/img_svr/scripts/Main.sh
      /opt/GRamp/apache2/htdocs/img_svr/scripts/WEB_TILE.sh (web_view function)
       /opt/GRamp/apache2/htdocs/img_svr/scripts/tilemaker.py


Web Tile Directories
++++++++++++++++++++
    
    - Input dir:

      + /raid/DGETS_DATA/INPUT
   
    - Processing dir:

      + /raid/DGETS_DATA/PROCESSING

    - Output dir: ???

      + /raid/DGETS_DATA/DONE

    - JPG dir:
    
      + /raid/DGETS_DATA/MISSIONS/MISSIONID/SCENE.dir/VIEWER_FILES/tile-%d-%d-%d-%d.jpg

.. information::

   what is /raid/DGETS_DATA/MISSIONS/MISSIONID/SCENE.dir/OV.jpg?


JPEG2000
--------

.. note::
   
   Processing handled by S98P3I_PROC which calls:
     /opt/GRamp/apache2/htdocs/img_svr/scripts/Main.sh
      /opt/GRamp/apache2/htdocs/img_svr/scripts/TILE_NITF.sh
       /opt/GRamp/apache2/htdocs/img_svr/scripts/libGOOGLE_OV.sh

.. note::

   JPIP server handled in S99SYERS
    /usr/local/bin/kdu_server_start.csh       

JP2 Directories
++++++++++++++++++++
    
    - Input dir:

      + /raid/DGETS_DATA/INPUT
   
    - Processing dir:

      + /raid/DGETS_DATA/PROCESSING

    - Output dir:

      + /raid/DGETS_DATA/MISSIONS/MISSIONID/SCENE.dir/scene.jp2
