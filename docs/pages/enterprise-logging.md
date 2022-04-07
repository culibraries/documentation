# Enterprise Logging

The logging stack consists of AWS Open Search , OpenSearch Dashboards, and Fluent Bit. 

## Installation

The installation was directly copied from [AWS EKS Workshop](https://www.eksworkshop.com/intermediate/230_logging/) with one exception. The first item is to create an OIDC identity provider. This had already been done when installing the AWS load balancer.

## Components

1. [Fluent Bit](https://fluentbit.io/): an open source and multi-platform Log Processor and Forwarder which allows you to collect data/logs from different sources, unify and send them to multiple destinations. It’s fully compatible with Docker and Kubernetes environments.
1. [Amazon OpenSearch Service](https://aws.amazon.com/opensearch-service/): OpenSearch is an open source, distributed search and analytics suite derived from Elasticsearch. Amazon OpenSearch Service offers the latest versions of OpenSearch, support for 19 versions of Elasticsearch (1.5 to 7.10 versions), and visualization capabilities powered by OpenSearch Dashboards and Kibana (1.5 to 7.10 versions).
1. [OpenSearch Dashboards](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/dashboards.html): OpenSearch Dashboards, the successor to Kibana, is an open-source visualization tool designed to work with OpenSearch. Amazon OpenSearch Service provides an installation of OpenSearch Dashboards with every OpenSearch Service domain.

## CU Boulder Initial Domain

[OpenSearch Dashboards URL](https://search-cubl-logging-pqdvd2kc3sh3tdlidfc26xnwba.us-west-2.es.amazonaws.com/_dashboards)

Username and Password in Keepass

[Domain endpoint](https://search-cubl-logging-pqdvd2kc3sh3tdlidfc26xnwba.us-west-2.es.amazonaws.com)

### Issues

1. Multiline logs are split into single line logs. Therefore, we need to configure the parser for multiline logs within Fluent Bit.