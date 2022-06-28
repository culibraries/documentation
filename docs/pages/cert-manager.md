# Cert Manager

All CTA Infrastructure & Applications certificates are issued with the cert-manager application in the production kubernetes cluster.

## DNS Registration

1. CU Boulder OIT has CNAMEs that are directed to our AWS Application Load Balancer
    * cubl-load-balancer (arn:aws:elasticloadbalancing:us-west-2:735677975035:loadbalancer/app/cubl-load-balancer/3039b8466406df2c)
    * If deleted will have to register all domains with CU Boulder OIT and point all CNAMEs to new load balancer

##  Network Configuration

Certificates are held within a secret on production cluster. The following network configuration is important to keep Target groups up to date with current Worker nodes in the cluster. If target group does not have a worker nodes the certificate will fail.

### ALB Listeners

1. Http: 80
    * RULEs
        Path is /.well-known/acme-challenge/* ====> k8s-nodes 
        Otherwise redirect Https(443) 
1. Https: 443
    * Rules are setup for each domain. Test domains are usually locked to campus and VPN IPs.

### Target Groups

1. http 80 => k8s-nodes
1. https 443 => k8s-nodes-https

## Lets Encrypt Certificate and Issuer

* Github repository for certificates and issuer yaml file. [culibraries/cert-manager](https://github.com/culibraries/cert-manager)
* Daily Cronjob runs to update certificate in AWS Certificate Manager. [culibraries/k8s-cronjob-tasks](https://github.com/culibraries/k8s-cronjob-tasks/tree/main/cert-aws-upload)

## Example Certificate Request Failure

Recently, DNS (folio.colorado.edu) was transferred to the FOLIO team. As a result, my certificate request failed because the folio.colorado.edu domain name was routed to a different AWS Load Balancer.

### Corrective Actions

1. If you think your certificate is correct, check certificate request.

    ```sh
    kubectl get certificaterequest -n cert-manager
    NAME                              READY    AGE
    cubl-lib-colorado-edu-99lwk       True     5d16h
    cubl-lib-colorado-edu-9d98s       False    5d16h
    ...

    kubectl describe certificaterequest cubl-lib-colorado-edu-9d98s -n cert-manager
    ```

1. Update Certificate and Delete Failed Request [Update Certificate](https://github.com/culibraries/cert-manager/commit/a32dd9e2358c1848fa7156b36f3cde4d7d4faf42)

    ```sh
    kubectl delete certificaterequest cubl-lib-colorado-edu-9d98s -n cert-manager
    kubectl apply -f certs/cubl-lib-colorado-edu-main.yaml -n crontab
    ```
