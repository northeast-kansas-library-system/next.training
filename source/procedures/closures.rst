Procedures for a Next Search Catalog library closure
====================================================

The main responsibility of staff at the closed library is to contact NEKLS about the closure.

The information we need is:

- The name of your library
- The dates of the closure
- Your contact information
- The reason for the closure

The best ways to contact us are:

- staff@nekls.org
- nexthelp@nekls.org
- courier@nekls.org
- Phone 785-838-4090 or (toll free) 888-296-6963 or (after hours) 785-813-1356


NEKLS staff will do the following:

1-2 day emergency closure
-------------------------

Expected results
^^^^^^^^^^^^^^^^

- Stop courier (if feasible)
- Inform other libraries (if necessary)
- Prevent undeserved late fees

Process for NEKLS staff
^^^^^^^^^^^^^^^^^^^^^^^

#. The Kansas Library Express Coordinator will determine whether or not to contact Henry Industries or other KLE members about the closure
#. The Next Search Catalog Coordinator will determine whether or not to contact other Next members about the closure
#. Next staff will change the calendar for the closed library in Koha
#. Next staff will run report 3171 entering dates affected by the closure as the start dates and ending dates (this report will list the item numbers of all items due during the date range you specify) - the SQL of this report is:

.. code-block:: sql
  :linenos:

  SELECT
    issues.branchcode,
    issues.itemnumber AS ITEM_NUMBER,
    issues.date_due
  FROM
    issues
  WHERE
    issues.branchcode
      LIKE <<Select library|ZBRAN>> AND
    issues.date_due BETWEEN
      <<Between the start of the day on date1|date>>  AND
      (<<the end of the day on date2|date>> + interval 1 day)
  GROUP BY
    issues.branchcode,
    issues.itemnumber
  ORDER BY
    issues.branchcode,
    issues.date_due

5. Next staff will download the report as a csv file and send it to ByWater Solutions via their support system at https://ticket.bywatersolutions.com/
#. Next staff will ask ByWater to change the due dates on all of the itemnumbers in the file to the day after the reopening


3-21 day emergency closure
--------------------------

Expected results
^^^^^^^^^^^^^^^^

- Stop courier
- Inform other libraries
- Prevent undeserved late fees
- Prevent new transfers to closed library
- Remove closed library from the holds queue
- Prevent ShareIt requests

Process for NEKLS staff
^^^^^^^^^^^^^^^^^^^^^^^

#. The KLE coordinator will contact Henry Industries to stop courier service for the duration of the closure
#. The KLE coordinator will contact other courier libraries to inform them of the disruption in service
#. The Next Search Catalog Coordinator will contact other Next members to inform them of the disruption in service
#. Next staff will mark the closure dates in the calendar for the closed library in Koha *and* in ShareIt
#. Next staff will remove library from the holds queue in Koha (system preference StaticHoldsQueueWeight)
#. Next staff will run report 3171 entering dates affected by the closure as the start dates and ending dates (this report will list the item numbers of all items due during the date range you specify) - the SQL of this report is:

.. code-block:: sql
  :linenos:

  SELECT
    issues.branchcode,
    issues.itemnumber AS ITEM_NUMBER,
    issues.date_due
  FROM
    issues
  WHERE
    issues.branchcode
      LIKE <<Select library|ZBRAN>> AND
    issues.date_due BETWEEN
      <<Between the start of the day on date1|date>>  AND
      (<<the end of the day on date2|date>> + interval 1 day)
  GROUP BY
    issues.branchcode,
    issues.itemnumber
  ORDER BY
    issues.branchcode,
    issues.date_due

7. Next staff will download the report as a csv file and send it to ByWater Solutions via their support system at https://ticket.bywatersolutions.com/
#. Next staff will ask ByWater to change the due dates on all of the itemnumbers in the file to the day after the reopening
#. Next staff will suspend any unfilled requests assigned for pickup at the closed library in order to prevent items from getting lost in transit

   To do this, Next staff will run report 3276 which will show all unfilled requests assigned for pickup at the affected library - the SQL of this report is:

.. code-block:: sql
  :linenos:

  SELECT
    Concat(
      '<a href="/cgi-bin/koha/circ/circulation.pl?borrowernumber=',
      reserves.borrowernumber,
      '#reserves" target="_blank">Open in new window</a>'
    ) AS LINK,
    Count(reserves.reserve_id) AS Count_reserve_id
  FROM
    reserves
  WHERE
    reserves.branchcode LIKE <<Choose your library|LBRANCH>> AND
    reserves.found IS NULL AND
    reserves.suspend = ""
  GROUP BY
    reserves.borrowernumber,
    reserves.branchcode,
    reserves.found,
    reserves.suspend
  ORDER BY
    reserves.borrowernumber

10. Once the report has been run, Next staff will click on the "Open in a new window" link for each line in the report and suspend the requests for pickup at the closed library until the day that the library anticipates they will re-open
#. Next staff will check this report as needed and repeat the process until the library reopens
#. After suspending requests, staff will add the following message to each patron's account as an OPAC message:

.. code-block:: txt

  All requests for pickup at CLOSEDLIBRARYNAME have been temporarily suspended due to an unexpected closure at that library.  The suspension will be lifted automatically when the library reopens.


More than 21-day or indefinite emergency closure
------------------------------------------------

Expected results
^^^^^^^^^^^^^^^^

- Stop courier
- Inform other libraries
- Prevent undeserved late fees
- Prevent new transfers to closed library
- Remove closed library from the holds queue
- Hide closed library materials from the OPAC
- Redirect patron requests to a nearby library
- Prevent ShareIt requests

Process for NEKLS staff
^^^^^^^^^^^^^^^^^^^^^^^

1. The KLE coordinator will contact Henry Industries to stop courier service for the duration of the closure
#. The KLE coordinator will contact other courier libraries to inform them of the disruption in service
#. The Next Search Catalog Coordinator will contact other Next members about the closure
#. Next staff will mark the closure dates in the calendar for the closed library in Koha *and* in ShareIt
#. Next staff will remove library from the holds queue in Koha (system preference StaticHoldsQueueWeight)
#. Next staff will run report 3171 entering dates affected by the closure as the start dates and ending dates (this report will list the item numbers of all items due during the date range you specify) - the SQL of this report is:

.. code-block:: sql
  :linenos:

  SELECT
    issues.branchcode,
    issues.itemnumber AS ITEM_NUMBER,
    issues.date_due
  FROM
    issues
  WHERE
    issues.branchcode
      LIKE <<Select library|ZBRAN>> AND
    issues.date_due BETWEEN
      <<Between the start of the day on date1|date>>  AND
      (<<the end of the day on date2|date>> + interval 1 day)
  GROUP BY
    issues.branchcode,
    issues.itemnumber
  ORDER BY
    issues.branchcode,
    issues.date_due

7. Next staff will download the report as a csv file and send the csv file to ByWater Solutions via their support system at https://ticket.bywatersolutions.com/
#. Next staff will ask ByWater to change the due dates on all of the itemnumbers in the file to the day after the reopening
#. After ByWater updates the due dates, Next staff will inform the library
#. Next staff will suspend any unfilled requests assigned for pickup at the closed library and leave a message on the patron's account informing them of the reason the requests are being suspended
#. Next staff will suspend any unfilled requests assigned for pickup at the closed library in order to prevent items from getting lost in transit

   To do this, Next staff will run report 3276 which will show all unfilled requests assigned for pickup at the affected library - the SQL of this report is:

.. code-block:: sql
  :linenos:

  SELECT
    Concat(
      '<a href="/cgi-bin/koha/circ/circulation.pl?borrowernumber=',
      reserves.borrowernumber,
      '#reserves" target="_blank">Open in new window</a>'
    ) AS LINK,
    Count(reserves.reserve_id) AS Count_reserve_id
  FROM
    reserves
  WHERE
    reserves.branchcode LIKE <<Choose your library|LBRANCH>> AND
    reserves.found IS NULL AND
    reserves.suspend = ""
  GROUP BY
    reserves.borrowernumber,
    reserves.branchcode,
    reserves.found,
    reserves.suspend
  ORDER BY
    reserves.borrowernumber

12. Once the report has been run, Next staff will click on the "Open in a new window" link for each line in the report and suspend the requests for pickup at the affected library until the day that the library anticipates they will re-open
#. After suspending requests, staff will add the following message to each patron's account as an OPAC message:

.. code-block:: txt

  All requests for pickup at LIBRARYNAME have been temporarily suspended due to an unexpected closure at that library.  The suspension will be lifted automatically when the library reopens.

14. Next staff will modify the drop-down in the OPAC so that patrons cannot select the closed library as a pickup location for new requests

    To do this, staff need to add the following jQuery to the OPACUserJS system preference:

.. code-block:: java

  $("option[value='CLOSEDBRANCHCODE']").attr("value","NEWBRANCHCODE").html('BRANCHNAME: Closed to requests until REOPENINGDATE');

15. Next staff will modify the drop-down in the staff client so that staff cannot select the closed library as a pickup location for new requests

    To do this, staff need to add the following jQuery to the IntranetuserJS system preference:

.. code-block:: java

  $("#pickup option[value='CLOSEDBRANCHCODE']").attr("value","NEWBRANCHCODE").html('CLOSEDBRANCHNAME: Closed to requests until REOPENINGDATE.');

16. Add the branchcode from the home branch to the OpacHiddenItems system preference

    To do this, staff need to add the following line to the OpacHiddenItems system preference:

.. code-block:: txt

  homebranch: CLOSEDBRANCHCODE


Schools closed for summer vacation
--------------------------------

Change the calendar for that library in Koha
Reroute all requests to alternate library
Remove library from holds queue
Add jQuery to re-route all holds to new pickup library

Expected results
^^^^^^^^^^^^^^^^

- Stop courier
- Prevent new transfers to closed library
- Remove school library from the holds queue
- Hide school library materials from the OPAC
- Redirect patron requests to a nearby library
- Prevent ShareIt requests

Process for NEKLS staff
^^^^^^^^^^^^^^^^^^^^^^^

#. The KLE coordinator will contact Henry Industries to stop courier service until school resumes
#. Long before the end of the school year, Next staff will set the hard due dates in the school library's circulation rules so that all items are due the Friday before the last day of school
#. Before the end of the school year Next staff will mark the closure dates in the calendar for the closed library in Koha *and* in ShareIt
#. On the last day the school is open, Next staff will remove library from the holds queue in Koha (system preference StaticHoldsQueueWeight)
#. Next staff will modify the drop-down in the OPAC so that patrons the school libraries as pickup locations for new requests

    To do this, staff need to add the following jQuery to the OPACUserJS system preference:

.. code-block:: java

  $("option[value='PHAXTELL']").attr("value","SENECA").html('Prairie Hills - Axtell: Closed to requests until August');
  $("option[value='PHSES']").attr("value","SABETHA").html('Prairie Hills - Sabetha Elementary: Closed to requests until August');
  $("option[value='PHSHS']").attr("value","SABETHA").html('Prairie Hills - Sabetha High: Closed to requests until August');
  $("option[value='PHSMS']").attr("value","SABETHA").html('Prairie Hills - Sabetha Middle: Closed to requests until August');
  $("option[value='PHWAC']").attr("value","WETMORE").html('Prairie Hills - Wetmore: Closed to requests until August');

6. Next staff will modify the drop-down in the staff client so that staff cannot select the school libraries as pickup locations for new requests

    To do this, staff need to add the following jQuery to the IntranetuserJS system preference:

.. code-block:: java

  $("#pickup option[value='PHAXTELL']").attr("value","SENECA").html('Prairie Hills - Axtell: Closed to requests until August');
  $("#pickup option[value='PHSES']").attr("value","SABETHA").html('Prairie Hills - Sabetha Elementary: Closed to requests until August');
  $("#pickup option[value='PHSHS']").attr("value","SABETHA").html('Prairie Hills - Sabetha High: Closed to requests until August');
  $("#pickup option[value='PHSMS']").attr("value","SABETHA").html('Prairie Hills - Sabetha Middle: Closed to requests until August');
  $("#pickup option[value='PHWAC']").attr("value","WETMORE").html('Prairie Hills - Wetmore: Closed to requests until August');

7. Add the school branchcodes to the OpacHiddenItems system preference

    To do this, staff need to add the following line to the OpacHiddenItems system preference:

.. code-block:: txt

  homebranch: PHAXTELL, PHSES, PHSHS, PHSMS, PHWAC

8. Next staff will run report 2731 to determine which items are owned by the school libraries that are currently checked out at libraries other than the school libraries.
#. Once checked out items are identified, Next staff will place requests on those items for one of the NEKLS problem cards so that the items route to the NEKLS office instead of the school libraries during the closure.  All requests should have their "Not needed after" date set to the day the schools are scheduled to reopen.
