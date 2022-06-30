# Rancher (Original)

The original deployment of rancher was on EC2 instance. Single docker container running 2.2.x version of Rancher.

## Access TO EC2 Instance

1. Public Key(ec2-user@libops.colorado.edu)
1. Vida is currently the only CTA member

## System Operations

```sh
service-rancher
Usage: /opt/bin/service-rancher {status|start|stop|restart}
```

## Backup

The data volume for the EC2 instance has automatic snapshots for 15 days. 

## Administration

The production cluster is a single docker container that deployed a Rancher RKE cluster. If a node is terminated or has pressure(Disk/Memory/CPU), the node requires manual interaction. Spot instance terminated, delete the node in Rancher. If a node has pressure, you can download keys and ssh into the EC2 Instance. I find it easier to delete the node and generate a replacement.

## Historical Error and Resolution

### Local Certs Expired

The Rancher UI did not come up and kept restarting itself due to accessing the API server under localhost:6443.

#### Error

```sh
localhost:6443: x509: certificate has expired or is not yet valid
```

#### Solution

* Rancher Single Node setup on libops.colorado.edu
* Rancher docker container was running v2.2.8 at the time the error was generated
* Container had local file folder mount -v /opt/rancher/etcd:/var/lib/rancher

    ```sh
    $ sudo su -
    $ cd /opt/rancher/etcd/management-state/tls/
    # Check expiration of cert
    $ openssl x509 -enddate -noout -in localhost.crt
      notAfter=Apr  8 17:27:09 2020 GMT
    $ mv localhost.crt localhost.crt_back
    $ exit

    $ service-rancher restart
    ```

* The docker container restarted and the system updated the certificate. I also updated the container to v2.2.10.

#### Notes

This error was difficult to track down. Found solution at the end of this [https://github.com/rancher/rancher/issues/20011#issuecomment-608440069 Rancher/Rancher Issues]. The issue resulted in about 12 hours of downtime from the single node rancher deployment. The Kubernetes production and test clusters continued to run without interruption.

### Unable to add etcd peer to cluster

This error occurred within the test cluster when a spot instance was terminated.

#### Rancher Error within UI

* Failed to reconcile etcd plane: Failed to add etcd member [xxx-xxx-xxx-xxx] to etcd cluster

#### Solution

* Logged into Kubernetes Node
* Rancher UI Cluster > Nodes ... Download Keys

```sh
ssh -i id_rsa rancher@<ip of node>
docker logs etcd
...
2020-04-16 02:37:26.327849 W | rafthttp: health check for peer 5f0cd4c2c1c93ea1 could not connect: dial tcp 172.31.30.190:2380: i/o timeout (prober "ROUND_TRIPPER_SNAPSHOT")
...

docker exec -it etcd sh
etcdctl member list
5f0cd4c2c1c93ea1, started, etcd-test-usw2b-spot-1, https://172.31.30.190:2380, https://172.31.30.190:2379,https://172.31.30.190:4001
88d3ad844b3306a5, started, etcd-test-usw2c-spot-1, https://172.31.11.25:2380, https://172.31.11.25:2379,https://172.31.11.25:4001
b0c2cb2c8e55611f, started, etcd-test-usw2c-spot-2, https://172.31.4.219:2380, https://172.31.4.219:2379,https://172.31.4.219:4001
efb9f597e4952edb, started, etcd-test-usw2c-spot-3, https://172.31.8.159:2380, https://172.31.8.159:2379,https://172.31.8.159:4001 
```

The problem was that the spot node was terminated but the etcd cluster did not release the node. Checked Rancher UI for IPs and 172.31.30.190 was no longer available.

```sh
etcdctl member remove 5f0cd4c2c1c93ea1
Member 5f0cd4c2c1c93ea1 removed from cluster c11cbcba5f4372cf
etcdctl member list
88d3ad844b3306a5, started, etcd-test-usw2c-spot-1, https://172.31.11.25:2380, https://172.31.11.25:2379,https://172.31.11.25:4001
b0c2cb2c8e55611f, started, etcd-test-usw2c-spot-2, https://172.31.4.219:2380, https://172.31.4.219:2379,https://172.31.4.219:4001
efb9f597e4952edb, started, etcd-test-usw2c-spot-3, https://172.31.8.159:2380, https://172.31.8.159:2379,https://172.31.8.159:4001
```

Returned to UI and added new etcd instances. etcd nodes most always be an odd number. Nodes vote stuck in split-brain or etcd cluster had 4 nodes and was waiting on non-existent node to vote.

### Unable to mount volume

If all nodes are not in a subnet that contains a volume. Deployment will fail with volume mount.
