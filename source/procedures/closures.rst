Procedures for a Next Search Catalog library closure
====================================================

The main responsibility of the library director is to contact NEKLS about the closure.

The information we need is:

- The name of the library
- The dates of the closure
- The reason for the closure
- Your contact information

The best ways to contact us are:

- staff@nekls.org
- nexthelp@nekls.org
- courier@nekls.org
- Phone 785-838-4090 or (toll free) 888-296-6963 or (after hours) 785-813-1356


NEKLS staff will do the following:

1-2 day emergency closure
-------------------------

#. Kansas Library Express Coordinator will determine whether or not to contact Henry Industries or other KLE members about the closure
#. The Next Search Catalog Coordinator will determine whether or not to contact other Next members about the closure
#. Next staff will change the calendar for the closed library in Koha
#. Next staff will run report 3171 entering dates affected by the closure as the start dates and ending dates (this report will list the item numbers of all items due during the date range you specify - the SQL of this report is:

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

#. Next staff will download the report as a csv file and send the csv file to ByWater Solutions via their support system at https://ticket.bywatersolutions.com/
#. Next staff will ask ByWater to change the due dates on all of the itemnumbers in the file to the date of reopening (or a day or two later is OK, too)
#. After ByWater updates the due dates, Next staff will inform the library

3-21 day emergency closure
--------------------------

#. The Next Search Catalog Coordinator will contact other Next members about the closure

Temporarily discontinue courier service

Suspend all requests until day after reopening

#. The KLE coordinator will contact Henry Industries to stop courier service for the duration of the closure
#. The KLE coordinator will contact other courier libraries to inform them of the disruption in service
#. The Next Search Catalog Coordinator will contact other Next members about the closure
#. Next staff will change the calendar for the closed library in Koha
#. Next staff will remove library from the holds queue in Koha (system preference StaticHoldsQueueWeight)
#. Next staff will run report 3171 entering dates affected by the closure as the start dates and ending dates (this report will list the item numbers of all items due during the date range you specify - the SQL of this report is:

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

#. Next staff will download the report as a csv file and send the csv file to ByWater Solutions via their support system at https://ticket.bywatersolutions.com/
#. Next staff will ask ByWater to change the due dates on all of the itemnumbers in the file to the date of reopening (or a day or two later is OK, too)
#. After ByWater updates the due dates, Next staff will inform the library
#. Next staff will suspend any unfilled requests assigned for pickup at the closed library and leave a message on the patron's account informing them of the reason the requests are being suspended

#. Next staff will run report 3276 which will show all unfilled requests assigned for pickup at the affected library - the SQL of this report is:

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

#. Once the report has been run, Next staff will click on the "Open in a new window" link for each line in the report and suspend the requests for pickup at the affected library until the day that the library anticipates they will re-open
#. Next staff will check this report daily and repeat the process until the library reopens

.. ToDo - add notes to report 3276

#. After suspending requests, add the following message to each patron's account as an OPAC message (i.e. a message the patron can see) that says:

.. code-block:: txt

  All requests for pickup at LIBRARYNAME have been temporarily suspended to to an emergency closure at that library.  The requests will automatically resume when the library reopens.




More than 21-day or indefinite emergency closure
------------------------------------------------

Reroute all requests to alternate library
Remove library from holds queue
Add jQuery to re-route all holds to new pickup library


#. The KLE coordinator will contact Henry Industries to stop courier service for the duration of the closure
#. The KLE coordinator will contact other courier libraries to inform them of the disruption in service
#. The Next Search Catalog Coordinator will contact other Next members about the closure
#. Next staff will change the calendar for the closed library in Koha
#. Next staff will remove library from the holds queue in Koha (system preference StaticHoldsQueueWeight)
#. Next staff will run report 3171 entering dates affected by the closure as the start dates and ending dates (this report will list the item numbers of all items due during the date range you specify - the SQL of this report is:

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

#. Next staff will download the report as a csv file and send the csv file to ByWater Solutions via their support system at https://ticket.bywatersolutions.com/
#. Next staff will ask ByWater to change the due dates on all of the itemnumbers in the file to the date of reopening (or a day or two later is OK, too)
#. After ByWater updates the due dates, Next staff will inform the library
#. Next staff will suspend any unfilled requests assigned for pickup at the closed library and leave a message on the patron's account informing them of the reason the requests are being suspended

#. Next staff will run report 3276 which will show all unfilled requests assigned for pickup at the affected library - the SQL of this report is:

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

#. Once the report has been run, Next staff will click on the "Open in a new window" link for each line in the report and suspend the requests for pickup at the affected library until the day that the library anticipates they will re-open
#. Next staff will check this report daily and repeat the process until the library reopens

.. ToDo - add notes to report 3276

#. After suspending requests, add the following message to each patron's account as an OPAC message (i.e. a message the patron can see) that says:

.. code-block:: txt

  All requests for pickup at LIBRARYNAME have been temporarily suspended to to an emergency closure at that library.  The requests will automatically resume when the library reopens.

#. Next staff will add the following jQuery to the OPACUserJS system preference:

.. code-block:: java

  $("option[value='CLOSEDBRANCHCODE']").attr("value","NEWBRANCHCODE").html('BRANCHNAME: Closed to requests until REOPENINGDATE');

-. This code will push any requests that patrons try to make in the OPAC for pickup at the closed library to the secondary library until the closure is over.

School closed for summer vacation
--------------------------------

Discontinue courier service
Change the calendar for that library in Koha
Ask ByWater to push Koha due dates to date of reopening
Reroute all requests to alternate library
Remove library from holds queue
Add jQuery to re-route all holds to new pickup library
