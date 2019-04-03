# April 2019
## Monday 1st
* Completed patch to increase debugging for [MB-33423](https://issues.couchbase.com/browse/MB-33423). see [http://review.couchbase.org/#/c/106703](http://review.couchbase.org/#/c/106703/)
* Worked on [MB-33253](https://issues.couchbase.com/browse/MB-33253) appears to be a problem with when we flush a bucket.  Where EP ns_server calls a series of delete vbuckets followed by a series of create vbuckets. The DCP connections seem to remain intact and we do not clean-up the number of active/snoozing backfills correctly.

## Tuesday 2nd
* Contined work on [MB-33253](https://issues.couchbase.com/browse/MB-33253).  After talking with Jim understand the behaviour to be that we want to close all streams on a flush however the dcp connections can remain.  Worked on duplicating the issue with a exp_store_test that does the following:

``` 
1. Creates some items
2. Creates a new checkpoint
3. Create more items
4. Create another checkpoint - (now can remove initial checkpoint)
5. Remove closed unreferenced checkpoints
   (will result in producer requiring a backfill)
6. Create a DCP producer
7. Do a streamRequest of the producer to produce a stream.
   (This will result in the backfill being generated.)
8. Delete the vbucket
9. Re-create the vbucket - step 8 and 9 replicates what happens on a flush.
10. Repeat steps 1 to 5 so that another backfill will be required.
11. Repeat step 7.
```

