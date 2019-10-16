Procedures for a Next Search Catalog library closure
====================================================

The main responsibility of the library director is to contact staff@nekls.org about the closure.

The information we need is:

- The name of the library
- The dates of the closure
- The reason for the closure
- Your contact information


1-2 day emergency closure
-------------------------

#. Make sure staff@nekls.org has been informed of the closure
#. The Kansas Library Express Coordinator will determine whether or not to contact Henry Industries or other KLE members about the closure
#. The Next Search Catalog Coordinator will determine whether or not to contact other Next members about the closure
#. Change the calendar for the closed library in Koha
#. Run report 3171 entering dates affected by the closure as the start dates and ending dates (this report will list the item numbers of all items due during the date range you specify - the SQL of this report is:

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

#. Download the report as a csv file
#. Open a support request with ByWater Solutions at https://ticket.bywatersolutions.com/
#. Attach the csv file to the support request
#. Ask ByWater to change the due dates on all of the itemnumbers in the file to the date of reopening (or a day or two later is OK, too)
#. After ByWater updates the due dates, inform the library

3-21 day emergency closure
--------------------------

Temporarily discontinue courier service

Suspend all requests until day after reopening

#. Make sure staff@nekls.org has been informed of the closure
#. The KLE coordinator will contact Henry Industries to stop courier service for the duration of the closure
#. The KLE coordinator will contact other courier libraries to inform them of the disruption in service
#. The Next 
#. Change the calendar for the closed library in Koha
#. Remove library from the holds queue in Koha (system preference StaticHoldsQueueWeight)
#. Run report 3171 entering dates affected by the closure as the start dates and ending dates (this report will list the item numbers of all items due during the date range you specify - the SQL of this report is:

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

#. Download the report as a csv file
#. Open a support request with ByWater Solutions at https://ticket.bywatersolutions.com/
#. Attach the csv file to the support request
#. Ask ByWater to change the due dates on all of the itemnumbers in the file to the date of reopening (or a day or two later is OK, too)
#. After ByWater updates the due dates, inform the library


More than 21-day or indefinite emergency closure
------------------------------------------------

Temporarily discontinue courier service
Change the calendar for that library in Koha
Ask ByWater to push Koha due dates to date of reopening
Reroute all requests to alternate library
Remove library from holds queue
Add jQuery to re-route all holds to new pickup library


School closed for summer vacation
--------------------------------

Discontinue courier service
Change the calendar for that library in Koha
Ask ByWater to push Koha due dates to date of reopening
Reroute all requests to alternate library
Remove library from holds queue
Add jQuery to re-route all holds to new pickup library
