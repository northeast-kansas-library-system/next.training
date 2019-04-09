Holds queue information
=======================

Holds queue schedule
--------------------

The holds queue is re-built every day based on this schedule:

.. csv-table::
   :header: "Morning/daytime", "Evening/nighttime"
   :widths: 20, 20

   "7:52 a.m.", ""
   "8:52 a.m.","8:52 p.m."
   "9:52 a.m.",""
   "10:52 a.m.","10:52 p.m."
   "",""
   "12:52 p.m.","12:52 a.m."
   "",""
   "2:52 p.m.","2:52 a.m."
   "",""
   "4:52 p.m.","4:52 a.m."
   "",""
   "6:52 p.m.","6:52 a.m."
   "",""

Holds queue and availability
----------------------------

When a patron places a title level request and multiple copies of that title are available, Koha assigns a copy of that title to fill the request based on these criteria:

#. If a copy is available at the pickup location, that copy is always assigned to fill the request.
#. If 
