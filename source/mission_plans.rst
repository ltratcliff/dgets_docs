Mission Plans
=============

Data Flow
---------
 - Mission Plans comes in via AMQ and are processed by Process_Combined_Collection_Plan.pl:
   + from DGS1 -> ECH-DGETS 
     - AMQ Topics= $THREAD_DGETS_COMBINED_CSP_PLAN & $THREAD_CSP_SP_FILE
   + from DGS2 -> WCH-DGETS 
     - AMQ Topics= $THREAD_DGETS_COMBINED_CSP_PLAN & $THREAD_CSP_SP_FILE

.. note::
   Process_Combined_Collection_Plan.pl is combined in the gr-syersFF service.


Process_Combined_Collection_Plan listens on AMQ, Parses the COMBINED_CSP_PLAN
Message for Primary targets, Coords for footprints, With Targets and DPs in Nav.
Parsed data is inserted into the DGETS MYSQL db.

- /opt/GRamp/apache2/htdocs/img_svr/DGETS_REFACTOR_Z/PERL/Process_Combined_Collection_Plan.pl:

   - Nav Plan written to nav_plan table
   - Mission Plan sp, zipped and then inserted as blob to mission_plan table
   - Collection Plan data inserted into collection_plan table



Logs for Mission Plan ingest are located:
 - /raid/DGETS_DATA/dgets_logs/Mission_Plans_Ingest.out
  - Log indicates sending DGS, Processed Plan, Changes to Nav & SP

Restart Process_Combined_Collection_Plan.pl via:

  ``svcadm restart gr-syersFF``

   or via the admin_hud

 - Sometimes toggling AMQ on DGETS will help with stubborn mission plans being ingested
