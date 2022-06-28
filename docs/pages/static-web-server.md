# Static Web Server

The static web server is configured behind CU Boulder federated SSO.

## Configuration

1. Cybercom API handles the SAML Service Provider
1. [cubl_static](https://github.com/culibraries/cubl_static)
1. Deployment yaml provided within repostitory
1. Nginx uses DNS Resolver within Kubernetes cluster. New cluster will need to check the IP of kube_dns.

    ```sh
    cat /etc/resolv.conf 
    nameserver 10.43.0.10
    search prod-cybercom.svc.cluster.local svc.cluster.local cluster.local
    options ndots:5
    ```

1. Add nameserver ip to default config

## Current Applications

1. Static Web
1. Print Purchase
1. LibBudget
