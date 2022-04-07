# Digital Library Platform

The Digital Library Platform(DLP) is the project to replace LUNA. Samvera stack was selected with the [Oregon Digital](https://www.oregondigital.org/catalog?) application as a model for CU DLP application. 

Contact: Michael Dulock

## Initial Deployment

1. [Helm install from Samvera](https://github.com/samvera/hyrax/tree/main/chart/hyrax)

## Problems

1. AWS EFS fileshare 
    * Hyrax front-end and Hyrax-workers did not map uploads to the same path
    * The Hyrax container(front-end) had uploads in path `/app/samvera/uploadshyrax`
    * The Hyrax worker container had the uploads int path `/app/samvera/uploads`
    * This may be fixed with any updates to the Chart.