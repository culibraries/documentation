# PetaLibrary S3 Glacier 
Documentation for the Globus Online transfer of Petalibrary data to S3 Bucket.

## Configuration

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

## TODO

1. If trial successful add new globus endpoints
1. The CU Scholar archive is currently being manually moved. This would cut out the middle step and provide direct access from S3 to PetaLibrary.
1. Transfer S3 bucket(cubl-ir-fcrepo) ==> /pl/archive/libdigicoll/dataSets/cu_scholar/cubl-ir-fcrepo
1. The above actions will allow for 3 copies with one copy in a different geolocation. 
1. This is part of the Core Trust Seal actions needed for CU Scholar.
1. AWS Lambda to move IR files on demand