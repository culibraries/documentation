# IR CU Scholar Development

IR Development process and common problems with CU Scholar. Documents the current status and the 
Docs for development process.

Contact: Andrew Johnson

## Local Developement

The Samvera community has good documentation regarding local development and dependencies.

[Samvera Docs](https://github.com/samvera/hyrax/blob/main/documentation/developing-your-hyrax-based-app.md)

* Double Check: solr_wrapper and fcrepo_wrapper require ```java 1.8```

[Development Documentation](https://samvera.github.io/)

### Configuration
* Build local Gems

    ```sh
    bundle install
    ```

* Start local rails server

    ```sh
    bin/rails hydra:server
    ```
* Create local sqlite3 DB

    ```sh
    bin/rails db:migrate RAILS_ENV=development
    ```
* Create CU Scholar Admin User and Admin Sets

    ```sh
    build/firstrun.sh
    ```

After this process the Solr and FCRepo is setup along with local admin user.

## Groups and Permission

Admin user can update roles.

[https://scholar.colorado.edu/roles](https://scholar.colorado.edu/roles)

## Common Errors

## Permission errors with Solr

* User delete/upgrade version of main file. Updated file does not get the permissions set within solr.  

![Error Message](assets/error_scholar.png)

* Solution: Add test file and mark individual file as private 
* [example](https://scholar.colorado.edu/concern/datasets/k930bz133)

## Encoding Error

* This error is difficult to track down. If an illegal character(non-UTF-8). The Fedora Commons Repository stores the characters, but on the Solr side will not store correctly. Data will be different on the front end compared to the data in the edit form.

* Solution: Use editor, which shows encoding. I use Microsoft Code and copy each item to the editor and view encoding. Once found, update the form item. The Abstract field is usually the culprit.


## SOLR (Primary Concern)

The Solr instance needs to be even distributed cores for each node. Our current production cluster has two Cores with three nodes. I believe this is what is causing our problems. I have worked on the test cluster with multiple configurations. The original design was similar to ElastiSearch, which handles the multiple core configuration. I assumed that Solr Cloud handles this as well; I was wrong. The last configuration has only two replicas deployed with two cores on each node. It appears to work. I have not deployed to production.

### Example Configuration Changes

1. Create Backup of current collection
    ```sh
    curl "http://solr-svc:8983/solr/admin/collections?action=BACKUP&name=2022-03-14-testSolrBackup&collection=hydra-prod5&location=/backup/test"
    ```
2. Delete Collection
    ```sh
    curl "http://solr-svc:8983/solr/admin/collections?action=DELETE&name=hydra-prod5"
    ```
3. Perform configuration action
    ```sh
    kubectl -n scholar scale statefulsets solr --replicas=2
    ```
4. Restore from backup
    ```sh
    curl "http://solr-svc:8983/solr/admin/collections?action=RESTORE&name=2022-03-14-testSolrBackup&collection=hydra-prod5&location=/backup/test"
    ```
