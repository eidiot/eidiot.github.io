title: hsqldb on Mac OS X
date: 2014-07-26 13:29:46
tags:
---
It took me a while googling how to run [hsqldb](http://www.hsqldb.org/) on Mac OS X. It turned out very easy when I looked back at its runners for Windows:

``` bat runServer.bat
cd ..\data
@java -classpath ../lib/hsqldb.jar org.hsqldb.server.Server %1 %2 %3 %4 %5 %6 %7 %8 %9
```

Get it running on OS X just by typing it on Terminal, or create a runServer.sh file and run it anywhere.

```sh runServer.sh
cd path/to/hsqldb/data
java -cp ../lib/hsqldb.jar org.hsqldb.server.Server

```
