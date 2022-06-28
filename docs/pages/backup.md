# Data Backup Policy

CU Libraries Core Tech and Apps provides backup of all production applications. The policy changes with the format of the data. Production applications are deployed using AWS services.

## Data Files

Data files are backed up using the 3-2-1 rule.

* 3 – Keep 3 copies of any important file: 1 primary and 2 backups.
* 2 – Keep the files on 2 different media types to protect against different types of hazards.
* 1 – Store 1 copy offsite (e.g., outside your home or business facility). 

1. S3

AWS S3 is preferred storage for production data files. AWS s3 can be tuned to provide automatic replication.

2. Elastic Block Store

AWS EBS snapshots are held for a rolling 30 day period. This provides quick restoration of application components. For example the Solr index EBS blocks can be quickly restored for minimal downtime.

3. PetaLibrary

The PetaLibrary is a University of Colorado Boulder Research Computing service support the storage, archival, and sharing of research data.

CU Libraries stores copies of production data files as the offsite copy within the PetaLibrary. All other copies are located with the AWS infrastructure.

## Relational Database

Production Relational Databases are backed up daily via Amazon Web Services RDS. These backups are held for a rolling 30-day period. Additionally, AWS RDS is built on distributed, fault-tolerant, self-healing Aurora storage with 6-way replication to protect against data loss.
