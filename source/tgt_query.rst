Target Query
============

.. note::
   
   Target Query can be used to easily view S2 imagery via a Web Browser
   Can query imagery by BE, MissionID, Date, or Coords

Main Query Page
----------------

Webpage located:
 - /opt/GRamp/apache2/htdocs/img_svr/img_query/IMAGE_QUERY.html

JS located:
 - /opt/GRamp/apache2/htdocs/img_svr/img_query/js/grids/QUERY.js

PHP located:
 - /opt/GRamp/apache2/htdocs/img_svr/img_query/php/XML_QUERY_OUTPUT.php


Viewer Page
-----------

   - /opt/GRamp/apache2/htdocs/img_svr/TILES/index.php
   - /opt/GRamp/apache2/htdocs/img_svr/img_query/TILES/php/VIEWER_OUTPUT.php


Reference Tabs
---------------

JS located:
 - /opt/GRamp/apache2/htdocs/img_svr/img_query/TILES/js/grids/
   - main_viewer.js
   - current_img.js
   - products.js
   - dgets_ref.js
   - ipl_ref.js
   - gets_ref.js
   - ias_ref.js

PY located (called from XML_QUERY_OUTPUT):
 - /opt/GRamp/scripts/
   - uvdsToTmp.py
   - iplscrape.py
   - ias_scrape.py
   - gets_scrape.py
