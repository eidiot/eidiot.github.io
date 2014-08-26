title: Run different hsqldb databases
date: 2014-08-26 15:47:32
tags:
---
I have different projects using hsqldb with slightly different tables and I want to have different databases for them. After I found out how to run [hsqldb on Mac OS X](2014/07/26/hsqldb-on-Mac-OS-X/), it turns out quite straightforward to do so.

What the windows runner of hsqldb `runServer.bat` does is going to the `data` folder and run the server there. Then the server will create and save all data in this `data` folder.

``` bat runServer.bat
cd ..\data
@java -classpath ../lib/hsqldb.jar org.hsqldb.server.Server %1 %2 %3 %4 %5 %6 %7 %8 %9
```
So my new script make a new folder under each project named `db` or whatever and go into it then run the server. All projects can now have their own hsqldb databases.

```sh runServer.sh
mkdir db
cd db
java -cp path/to/hsqldb/lib/hsqldb.jar org.hsqldb.server.Server

```
I name this script `db.sh` and put it in my workspace then in each project I run `../db.sh`. If I do need some new porject have the same data to start with I only need to copy the db folder.
