---
layout: master
title: Postgres dump to CSV
keywords: postgres, csv, dump
description: Postgres dump database to CSV format
date: Oct 16 2010
permalink: /blog/postgres/postgres-dump-to-csv
---

I was recently having an issue doing a dump to a CSV file from psql command line.  Some bug with a permissions error.

The fix was to do run it from the shell like so:

~~~
$ psql -c "COPY (SELECT * FROM <schema>.<table>) TO STDOUT with CSV HEADER" 
       -o /path/to/dump.csv -U <username> <database>;
~~~

Putting that up more for my own reference then anything, but hope it helps someone else out there.