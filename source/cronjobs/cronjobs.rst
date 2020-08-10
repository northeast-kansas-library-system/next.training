Automatic processes running on our system
=========================================

.. _holds_queue:

Holds queue generation
----------------------

This script runs every day at:

.. csv-table:: Holds queue schedule
   :header: "Morning/daytime", "Evening/nighttime"
   :widths: 20, 20

   "6:52 a.m.","6:52 p.m."
   "7:52 a.m.","7:52 p.m."
   "8:52 a.m.","8:52 p.m."
   "9:52 a.m.","9:52 p.m."
   "10:52 a.m.","10:52 p.m."
   "11:52 a.m.","11:52 p.m."
   "12:52 p.m.","12:52 a.m."
   "1:52 p.m.","1:52 a.m."
   "2:52 p.m.","2:52 a.m."
   "3:52 p.m.","3:52 a.m."
   "4:52 p.m.","4:52 a.m."
   "5:52 p.m.","5:52 a.m."

We have a cron-job running on the server that re-builds the holds queue at regular intervals.

The purpose of this script for Next Search Catalog is to regenerate the pick-list at 1 hour intervals throughout the day.

The holds queue is randomized based on the following factors:

#. The holds queue is only randomized if multiple copies are available.
#. If an item is available at the request's pickup location that copy will always be assigned to fill the request regardless of the availability of any other copies.

The settings for this cron-job are:

.. code-block:: perl
   :linenos:

   52 */1 * * * nekls-koha $KOHA_CRON_PATH/holds/build_holds_queue.pl >/dev/null 2>&1

- 52 \*/1 * * * in the first line sets the schedule at 52 minutes past the hour for every hour of the day
- nekls-koha $KOHA_CRON_PATH/holds/build_holds_queue.pl tells the server which script to execute
- >/dev/null 2>&1 prevents the script from mailing an error log to the system administrator

Examples:
^^^^^^^^^

1. The holds queue is random so, on average, a book available at 4 libraries should appear on one of those library's holds list 25% of the time.  Since it's random, though, it's entirely possible for an item to randomly be assigned to the same holds list again, and again, and again, and again.
  Example:  a patron places a request on THE BRETHEREN by John Grisham to be picked up at HORTON.  Copies are available at BASEHOR, LEAVENWRTH, RICHMOND, and HIAWATHA.  When staff at LEAVENWRTH run the holds report at 8:00 a.m., their copy of this book is on their holds list.  But they can't find their copy of this title so when the holds queue is re-built at 8:52 a.m., the book is re-assigned to the RICHMOND holds queue.  But RICHMOND is closed today, so no one at RICHMOND pulls the request.  Since the holds queue ignores closed days, at 9:52 a.m., the request is re-assigned to RICHMOND again.  And then, again, at 10:52, the request is re-assigned to RICHMOND a third time.  Then at 12:52, the request is re-assigned to BASEHOR.  Every time the holds queue is re-built, the process is randomized.

2.  Any item attached to a bibliographic record can fill a title level request - even if its not the copy currently assigned to the request in the holds queue.
  Example:  a patron places a request on CODE OF THE WOOSTERS by P.G. Wodehouse to be picked up at OTTAWA.  Copies are available at BONNERSPGS, SENECA, and OVERBROOK.  A fourth copy is owned by EUDORA, but it is checked out and not available.  Staff at OVERBROOK print their holds list at 8:00 a.m.  At 8:05 a.m., a patron returns the EUDORA copy.  When the EUDORA copy is checked in, the hold will be triggered for the patron at OTTAWA.  When staff at OVERBROOK check in their copy, the request is not triggered because the EUDORA item is already en route to OTTAWA to fill the request.


.. _cron_empty_bibs:

Empty bibliographic record deletion
-----------------------------------

This script runs every Sunday morning at 2:20 a.m.

We have a cron-job running on the server that automatically deletes bibliographic records that were created more than two weeks ago but do not have any items attached to them.

The purpose of this script for Next Search Catalog is to remove empty bibliographic records in a timely manner so that we do not have empty records taking up space in the catalog and confusing staff or patrons.

This cron-job covers three situations for us:

#. In practice, when cataloging staff delete all of the items attached to a bibliographic record, they should also delete the bibliographic record - sometimes this does not happen.
#. Similarly, if cataloging staff add a bibliographic record to the catalog, they should add items to that record in a timely manner - but that does not happen in some circumstances.
#. It is also possible for the last item attached to a bibliographic record to be deleted automatically if the item has been marked as "Lost (more than 45 days overdue)" or "Missing (unable to locate on the shelf)" or "Withdrawn" for more than 13 months.

All three of these situations create circumstances where we have bibliographic records taking space in the catalog without any items attached to them.  Bibliographic records without items can be confusing for staff and patrons because a title is visible but there are no physical items available if anyone actually wants to use a copy of that title.

The settings for this cron-job are:

.. code-block:: perl
   :linenos:

   20 2 * * 0 nekls-koha $HOME/drop_empty_bibs.pl --days=14 --ignore_url --silent --update > /dev/null

- 20 2 * * 0 sets the schedule at every Sunday morning at 2:20 a.m.
- nekls-koha $HOME/drop_empty_bibs.pl tells the server which script to execute
- --days=14 tells the script to delete biblios that are more than 14 days old
- --ignore_url tells the script to ignore items that have a URL in biblioitems.url
- --silent tells the script to ignore error messages
- --update > /dev/null prevents the script from mailing an error log to the system administrator

Examples:
^^^^^^^^^

#. A bibliographic record exists for a 1975 paperback edition of DAY OF THE JACKAL by Frederic Forsythe.  The bibliographic record was added to the catalog on September 9, 2005.  Only one item is attached to this bibliographic record.  Staff at NEKLS delete the only item attached to this bibliographic record on February 6, 2019, and they forget to delete the bibliographic record when they delete the item.  This bibliographic record will be deleted at 2:20 a.m on Sunday, February 10, because the record is more than 14 days old and it has zero items attached to it.
#. Staff at NEKLS add a bibliographic record for a new edition of THE ROLLING STONE RECORD GUIDE on March 6, 2019.  If no item is attached to this record by 2:20 a.m. on March 24, the record will automatically be deleted because it is more than 14 days old and it has zero items attached to it.
#. The only copy of FEAR AND LOATHING IN LAS VEGAS by Hunter S. Thompson has was marked as "Lost (more than 45 days overdue)" on June 1, 2018.  Unless the item is found and checked in, the item will be automatically deleted on July 1, 2019.  On Sunday, July 7, 2019, at 2:20 a.m., the bibliographic record will automatically be deleted because the record is more than 14 days old and it has zero items attached to it.
