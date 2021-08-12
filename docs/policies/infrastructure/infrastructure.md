# Infrastructure

CU Libraries infrastructure is leveraging AWS Cloud Services. 

![Default Infrasturcture](infrastructure.jpg)

## Kubernetes

Production products are deployed in containers(micro-service) within a [kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/) cluster. Rancher is used for cluster management and deployments.

        1. Highly available 
        2. Horizontal Scalable: Products have the ability to scale up or down based on demand.

### AWS EKS

The production clusters are using [AWS EKS](https://aws.amazon.com/eks) infrastructure with compute nodes using AWS EC2 instances.

        AWS EKS runs the Kubernetes control plane across multiple Availability Zones,
        automatically detects and replaces unhealthy control plane nodes, and provides on-demand, 
        zero downtime upgrades and patching. EKS offers a 99.95% uptime SLA. At the same time, 
        the EKS console provides observability of your Kubernetes clusters so you can identify 
        and resolve issues faster.

