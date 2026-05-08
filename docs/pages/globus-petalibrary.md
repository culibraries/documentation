# PetaLibrary S3 Backups 

## Scholar Backup
Scholar's data files, stored in AWS S3 bucket object store, are copied to PetaLibrary (*/pl/archive/libdigicoll-2/dataSets/cu_scholar/cubl-ir-fcrepo/*) using rclone on a compute node.  This is setup as a nightly job script and scheduled with the Slurm job scheduler.  

User needs to have UC Boulder Research Computing Account with Duo two-factor authentication, along with being a member of the "dulockgrp" access group.  The group will give access to the folder "/pl/archive/libdigicoll-2" for Scholar backup.

The PetaLibrary Tier provides full data checksumming.

This completes the three copies backup, with one copy in a different geolocation.


## Globus Configuration  ❗Deprecated
**Research Computing discontinued it's trial licenses for the Globus S3 Connector.** 

Documentation for the Globus Online transfer of PetaLibrary data to S3 Bucket.

1. Discussion with Research Computing (Jason Armbruster) to set up Globus Online endpoint (cubl-petalibrary-archive).
1. RC set up a globus endpoint "S3 prototype CU Boulder Libraries"
1. When you open that collection you're going to get prompted to authenticate with Boulder Identikey first, then once that's successful, you'll have to also authenticate with an AWS key/secret pair for a user who has access to the S3 bucket.
1. User needs to have UC Boulder Research Computing Account with access to "dulockgrp" group. The group will give access to "libdigicoll"
1. Path /pl/archive/libdigicoll/ to access UC Boulder Library data

## Trial S3 Data Transfer

1. CTA (Vida) is currently awaiting UC Boulder Research Computing account with access to Library data.
1. Data Trial: Contact Michael Dulock

    ```sh
    /pl/archive/libdigicoll/libimage-bulkMove/
    (2TB)
    
    /pl/archive/libdigicoll/libstore-bulkMove/ 
    (4.4TB)

    /pl/archive/libdigicoll/libberet-bulkMove/RFS/DigitalImages/
    (Important)

    /pl/archive/libdigicoll/libberet-bulkMove/RFS/
    (23TB)
    ```
