Automatic processes running on our system
=========================================

.. _cron_empty_bibs:

Empty bibliographic record deletion
-----------------------------------

This script runs every Sunday morning at 2:20 a.m.

We have a cron-job running on the server that automatically deletes bibliographic records that were created more than two weeks ago but do not have any items attached to them.

The purpose of this script for Next Search Catalog is to remove empty bibliographic records in a timely manner so that we do not have empty records taking up space in the catalog and confusing staff or patrons.

This cron-job covers three situations for us.  In practice, when cataloging staff delete all of the items attached to a bibliographic record, they should also delete the bibliographic record - sometimes this does not happen.  Similarly, if cataloging staff add a bibliographic record to the catalog, they should add items to that record in a timely manner - but that does not happen in some circumstances.  It is also possible for the last item attached to a bibliographic record to be deleted automatically if the item is overdue for more than 45 days.  All three of these situations create circumstances where we have bibliographic records taking space in the catalog without any items attached to them.  Bibliographic records without items can be confusing for staff and patrons.

The settings for this cron-job are:

20 2 * * 0 nekls-koha $HOME/drop_empty_bibs.pl --days=14 --ignore_url --silent --update > /dev/null

- 20 2 * * 0 sets the schedule at every Sunday morning at 2:20 a.m.
- nekls-koha $HOME/drop_empty_bibs.pl tells the server which script to execute
- --days=14 tells the script to delete biblios that are more than 14 days old
- --ignore_url tells the script to ignore items that have a URL in biblioitems.url
- --silent tells the script to ignore error messages
- --update > /dev/null prevents the script from mailing an error log to the system administrator
