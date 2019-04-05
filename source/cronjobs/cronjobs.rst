Automatic processes running on our system
=========================================


Empty bibliographic record deletion
-----------------------------------

We have a cron-job running on the Koha server called drop_empty_bibs.pl.  Our settings for this cron-job are:

20 2 * * 0 nekls-koha $HOME/drop_empty_bibs.pl --days=14 --ignore_url --silent --update > /dev/null

The purpose of this cron-job is to delete bibliographic records with 0 attached items.  It is scheduled to run every Sunday morning at 2:20 a.m. and it will delete any bibliographic records that have had 0 items attached to them for more than 14 days 



If a bibliographic record is added to Next Search Catalog but no items are attached to that bibliographic record, that record is considered an "Empty bibliographic record."  If a bibliographic record is empty for more than 14 days, it will be deleted on the following Sunday
