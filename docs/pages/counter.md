# COUNTER

**COUNTER** stands for Counting Online Usage of NeTworked Electronic Resources. It
is both a standard and the name of the governing body responsible for publishing
the related code of practice (CoP). The governing body represents a collaborative
effort of publishers and librarians whose collective goal is to develop and
maintain the standard (CoP) for counting the use of electronic resources in
library environments.

This document refers to the online tool developed by Libraries IT that simplifies
the aggregation and reporting of electronic resource usage data for University
Libraries.

## Overview of Loading Process
The following describes the steps to load COUNTER data from Excel spreadsheets. These
spreadsheets, or reports, are downloaded from the various platform sites by e-resources
staff and are stored on the Q: drive (typically Q:\SharedDocs\Usage Stats) in year-specific
folders. The product development team is notified by email that new reports are available
for processing and importing into the COUNTER database.

Workflow steps:

1. Copy new reports to a local working directory.
2. Run preprocessing/renaming script.
3. Copy renamed reports to the staging server.
4. Replicate production database on staging.
5. Run loading script.
6. Restore production database from staging.
7. Archive reports to AWS S3.

Each of these steps will be described in further detail later.

## Staging Infrastructure
The COUNTER staging infrastructure consists of an EC2 instance (counter-staging) with MySQL 5.7
installed. This approach removes the loading workload to AWS from your local desktop/laptop.
The staging server also facilitates access to both the test and production databases in RDS.
Details of the instance can be found in the AWS console.

Loading scripts and associated modules can be copied to the staging server by cloning the Github
repo (assuming you are starting at the home directory):

    $ git clone https://github.com/culibraries/counter-data-loader.git

All data (including the MySQL database) is stored on an attached volume (/dev/sdf) currently
sized at 50 GiB.

## Database Schema
![COUNTER ERD](assets/counter-erd.png)

## Details of the Loading Process
### Copy New Reports to Local Working Directory
Clone the repo on your local machine. Make a working directory called 'reports' in your
cloned repo:

    $ mkdir reports

Then copy all files to be processed from the Q: drive to this working directory.

### Run Preprocessing/Renaming Script
Run the following command:

    $ python preprocess-source-files.py

This script will copy and rename all files in the working directory into a temporary
directory (reports\tmp). Refer to the comments in the code for a description of the format.

If errors are raised, they will be recorded in an error log.

### Copy Renamed Reports to the Staging Server
Assuming that the previous step was successful, copy the renamed Excel files to the staging
server using FTP or SSH file transfer. The files should be copied to /data/counter.

### Replicate Production Database on Staging
The starting point for loading new COUNTER reports is the current production database. To
replicate the production database on staging, run the following commands:

    $ mysqldump counter5 -h cudbcluster.cluster-cn8ntk7wk5rt.us-west-2.rds.amazonaws.com
    -u dbmuser -p --add-drop-database -r /data/backups/20220329-counter-prod.sql
    $ mysql counter5 -u dbmuser -p < /data/backups/20220329-counter-prod.sql

When prompted, enter the password for dbmuser (available in KeePass).

NOTE: Use the current date-time stamp as the file prefix.

To improve loading performance, drop all indexes:

    $ mysql counter5 -u dbmuser -p < sql/drop-indexes.sql

At this point, the staging database is ready for loading the new files.

### Run Loading Script
The loading process is a multistep process:

* Read the title and usage data in the source Excel spreadsheet.
* Generate CSV files from the spreadsheet representing title information and corresponding metrics.
* Import CSV files into temporary tables.
* Do inserts/updates in title and metric tables.
* Log the spreadsheet as processed.

This is an iterative process that is performed for every spreadsheet to the loaded.

The entire sequence of steps as outlined above are initiated and executed from a single "controller"
file (loader.py). The process is started by entering the following command:

    $ python loader.py <report directory> <year>

The report directory parameter is the location of the prepared Excel files. The year parameter is
the 4-digit year that corresponds to the usage data, e.g., for a report containing usage data for
2021, this parameter value would be "2021" (without the quotes).

Refer to the source code comments for further details.

### Restore Database to Production
Once all spreadsheets have been loaded, the database on the staging server can be restored to
the production environment. Before doing this, though, the various database indexes must be
recreated. To do this, run the following command:

    $ mysql counter5 -u dbmuser -p < sql/create-indexes.sql

After the indexes have been recreated, the database can be restored to production:

    $ mysqldump counter5 -u dbmuser -p --add-drop-database -r /data/backups/20220329-counter-staging.sql
    $ mysql counter5 -h cudbcluster.cluster-cn8ntk7wk5rt.us-west-2.rds.amazonaws.com -u dbmuser
    -p < /data/backups/20220329-counter-staging.sql

### Archive Reports to AWS S3
The last step in the process is to archive all of the processed spreadsheets by moving them to AWS S3.
Do this by running the following command:

    $ aws s3 mv /data/counter/ s3://cubl-backup/counter-reports/ --recursive --storage-class ONEZONE_IA

## Other Considerations
### When Errors Occur During Loading
The loading process will raise an error (and log it) if the source spreadsheet cannot be loaded. Errors
typically occur when the spreadsheet does not adhere to the COUNTER specification. For example, sometimes
there will be a blank row at the top of the spreadsheet.

In these cases, the loading script will skip the spreadsheet and move on to the next one in the queue.
An entry will also be written to a log file. After loading has finished, these Excel files should be
examined for any obvious formatting errors and, if found, these can be rectified and the loading
script rerun. If errors persist, let the Product Owner know.

### Updating the platform_ref Table


### Making Backups
