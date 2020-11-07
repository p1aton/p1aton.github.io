Python
---

* lambda x: x+1 function + apply() : apply the lambda function on the x representing column

* lambda x: x+1 function + map() : map the lambda function on the x representing each element of the column

* applymap(): apply the function on each element of the entire dataframe

### Woring with time

* **The Epoch** in UNIX system is `January 1st, 1970` midnight. On other systems use the command `time.gmtime(0)` to find out to discover the date of the Epoch.

* time can be stored in (1)seconds since **the Epoch**, e.g. run command `time.time()` gives 1507235128.682  (2) as a tuple showing arranged the following way: year, month 1-12, day 1-31, hour 0-23, minutes 0-59, seconds 0-61, day 0-6 (Mon-Sun), day 1-366, and DST -1,0,+1. "DST" means "daylight savings time."  run by command 'time.gmtime(0)' (3) as a string format showing "Mon Feb 16 16:04:25 2004" run by command `time.ctime()`



