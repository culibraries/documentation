# GeoLibrary Development

CU GeoLibary is deploy from the [GeoBlacklight](https://geoblacklight.org/) open-source project. 

Contact Phil White

## Code Repositories

1. GeoLibrary [https://github.com/culibraries/geo-geolibrary](https://github.com/culibraries/geo-geolibrary)
2. GeoDataLoader [https://github.com/culibraries/geo-data-loader](https://github.com/culibraries/geo-data-loader)
3. geoBlacklightq [https://github.com/culibraries/geo-blacklightq](https://github.com/culibraries/geo-blacklightq)
4. GeoServer:A docker container that runs GeoServer influenced by this [docker recipe](https://github.com/eliotjordan/docker-geoserver/blob/master/Dockerfile)

## Local Development

1. [GeoBlacklight Guides](https://geoblacklight.org/guides.html)
1. GeoBlacklight online [Tutorial](https://geoblacklight.org/tutorials.html)
1. With the dependency with GeoServer, Solr, and Data Loader, I have found it easier to build code and deploy it to the Test cluster. [Dockerfile](https://github.com/culibraries/geo-geolibrary/blob/master/Dockerfile)

1. Celery, GeoServer,  and Data Loader application mount AWS EFS fileshare. This is the data store for geosever and for the application to load data into the Geoserver application.
