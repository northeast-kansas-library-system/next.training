Procedures for a Next Search Catalog library closure for health reasons
=======================================================================

The main responsibility of staff at the closed library is to contact NEKLS about the closure.

Even though your library will be closed to the public, we would like your library to coordinate receiving courier materials for 1 full week after the closure begins.  The reason for this is that it will be impossible for us to prevent courier items that are already in transit to your library from being shipped to your library.  We fear that because of the size of the courier system, if you stop receiving items immediately, the items that are already en route to your library may become lost permanently.

The information we need is:

- The name of your library
- The date you expect to re-open
- Whether or not you will be able to continue courier service for 1 week after the closure begins
- Your contact information
- The reason for the closure

The best ways to contact us are:

- nekls.staff@nekls.org
- nexthelp@nekls.org
- courier@nekls.org
- Phone 785-838-4090 or (toll free) 888-296-6963 or (after hours) 785-813-1356


NEKLS staff will do the following:

Expected results
^^^^^^^^^^^^^^^^

- Stop courier
- Inform other Next Search Catalog libraries
- Prevent undeserved late fees
- Prevent new transfers to closed library
- Remove closed library from the holds queue
- Hide closed library materials from the OPAC
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
#. Next staff will ask ByWater to change the due dates on all of the itemnumbers in the file to a day 7 days after your re-opening
#. After ByWater updates the due dates, Next staff will inform the library
#. Next staff will suspend any unfilled requests assigned for pickup at the closed library and leave a message on the patron's account informing them of the reason the requests are being suspended

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
