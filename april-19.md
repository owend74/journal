# April
## Monday 1st
* Completed patch to increase debugging for [MB-33423](https://issues.couchbase.com/browse/MB-33423). see [http://review.couchbase.org/#/c/106703](http://review.couchbase.org/#/c/106703/)
* Worked on [MB-33253](https://issues.couchbase.com/browse/MB-33253) appears to be a problem with when we flush a bucket.  Where EP ns_server calls a series of delete vbuckets followed by a series of create vbuckets. The DCP connections seem to remain intact and we do not clean-up the number of active/snoozing backfills correctly.