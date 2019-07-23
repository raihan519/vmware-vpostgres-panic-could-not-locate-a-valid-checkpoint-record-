# Error VMware vPostgres (PANIC : could not locate a valid checkpoint record)
This is a complete step to troubleshoot vPostgres error (PANIC : could not locate a valid checkpoint record) in vCenter 6.7.

## Cause of The Error
Based on my experience, this error happen after my vCenter harddisk disconnect suddenly. After i reconnect the harddisk to my vCenter, everything seems fine on the vCenter DCUI, but everything become not fine when i find out my vCenter web client service cannot start hehe.

**Questions** : So what is exactly cause this error?

**Answer** : The problem i encountered (HDD disconnect suddenly) is one of the reason that causing this error. But other than that, the reason of this error can be caused by a several case, like vCenter didn't shutdown correctly or maybe the part of the vPostgres data in the harddisk is corrupted so the vPostgres cannot load it's data.

## How Did I Find Out?
How did i know that "this kind of error" we talked about is the error i encounter?

**First** thing i did when my **vPostgres** cannot running was start the **vPostgres** manually, with hope **vPostgres** can start normally as usual. Unfortunately, my **vPostgres** service still cannot run.

You can inspect the file **failed-vpostgres.txt** in this repository to see the result when i'm trying to start vPostgres from CLI.

**Second**, i check the **vpxd.log** file in **/var/log/vmware/vpxd/** directory with cat command. But i found 1 line from the vpxd.log that doesn't make sense for me, it stated that the vCenter cannot start vPostgres because vCenter cannot connect to vPostgres server.

**Questions** : Why it doesn't make sense for me?

**Answer** : Both vCenter & vPostgres embedded in 1 server, that mean they are one unity, so it doesn't make when ther cannot connect one to another.

You can inspect the file **vpxd-log.txt** in this repository to see the content of the vpxd.log, include with the command to see the log file directly from CLI.

**Third**, last thing i did to find out what is my problem is check directly to **postgresql.log** in **/storage/db/vpostgres/pg_log/** directory. 1 line from the **postgresql.log** is stated that the vPostgres cannot find last database checkpoint when vPostgres last run.

You can inspect the file **postgres-log.txt** in this repository to see the content of the postgres.log, include with the command to see the log file directly from CLI.

That is how i know the error i encounter, and when i know the error that mean i know what should i ask to the forum about the error i encounter

## How to fix it
Actually, its pretty easy to solve this issue, you just need to reset vPostgres log. Run these 3 command from CLI with root credential and it's password.
```
cd /opt/vmware/vpostgres/current/bin

su vpostgres -s /bin/sh

/opt/vmware/vpostgres/current/bin/pg_resetxlog -f /storage/db/vpostgres
```
Done, that is how i fix the error.
