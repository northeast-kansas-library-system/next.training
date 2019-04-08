Automatic processes running on our system
=========================================

.. _holds_queue:

Holds queue generation
----------------------

This script runs every day at:

- 6:52 a.m.
- 7:52 a.m.
- 8:52 a.m.
- 9:52 a.m.
- 10:52 a.m.
- 12:52 p.m.
- 2:52 p.m.
- 4:52 p.m.
- 6:52 p.m.
- 8:52 p.m.
- 10:52 p.m.
- 12:52 a.m.
- 2:52 a.m.
- 4:52 a.m.

We have a cron-job running on the server that re-builds the requests pick-list at regular intervals.

The purpose of this script for Next Search Catalog is to regenerate the pick-list regularly so that requests for materials are spread amongst the libraries randomly.

ADDITIONAL INFORMATION - paragraph

The settings for this cron-job are:

.. code-block:: perl
   :linenos:

   52 */2 * * * nekls-koha $KOHA_CRON_PATH/holds/build_holds_queue.pl >/dev/null 2>&1
   52 7,9 * * * nekls-koha $KOHA_CRON_PATH/holds/build_holds_queue.pl >/dev/null 2>&1

- 52 \*/2 * * * in the first line sets the schedule at 52 minutes past the hour for every even numbered hour
- 52 7,9 * * * in the second line sets the schedule to also include 52 minutes past the hour at 7:00 a.m. and 9:00 a.m.
- nekls-koha $KOHA_CRON_PATH/holds/build_holds_queue.pl tells the server which script to execute
- >/dev/null 2>&1 prevents the script from mailing an error log to the system administrator





.. _cron_empty_bibs:

Empty bibliographic record deletion
-----------------------------------

This script runs every Sunday morning at 2:20 a.m.

We have a cron-job running on the server that automatically deletes bibliographic records that were created more than two weeks ago but do not have any items attached to them.

The purpose of this script for Next Search Catalog is to remove empty bibliographic records in a timely manner so that we do not have empty records taking up space in the catalog and confusing staff or patrons.

This cron-job covers three situations for us.  In practice, when cataloging staff delete all of the items attached to a bibliographic record, they should also delete the bibliographic record - sometimes this does not happen.  Similarly, if cataloging staff add a bibliographic record to the catalog, they should add items to that record in a timely manner - but that does not happen in some circumstances.  It is also possible for the last item attached to a bibliographic record to be deleted automatically if the item is overdue for more than 45 days.  All three of these situations create circumstances where we have bibliographic records taking space in the catalog without any items attached to them.  Bibliographic records without items can be confusing for staff and patrons.

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
