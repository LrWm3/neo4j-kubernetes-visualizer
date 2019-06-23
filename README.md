# neo4j-kubernetes

Import kubernetes resouces into neo4j, to visualize the cluster configuration. Goals are to visualize network request
flows throughout the cluster as well as kafka topic flows, while presenting the cluster in a way which matches the 
cluster layout.

## Goals

Visualize kubernetes resources using neo4j, maybe using bloom?

* Single cluster support; dont bother trying to support multiple clusters right away
* Periodically import latest kubernetes cluster resources into neo4j database
* Visualize pods running in neo4j database
* Visualize services routing traffic to pods 
* Visualize ingress routing traffic to services
* Visualize kafka topic inputs / outputs from pod configurations
* Cluster pods together based on deployments, and connect services to deployments
* Cluster pods together based on stateful-sets, and connect services to `<podname>-0`
* Cluster namespaced resources together based on if they are within that namespace
* Show direction of dataflow for kafka topics
* Size connections between pods / deployments based on flow through subscribed kafka topics
* Size connections between ingresses / services / pods based on volumes of network traffic 
* For http requests; display request response traffic from outside cluster as a different color; have it start at the ingress-controller pod

## TODO

Features I'm looking to complete:

- [ ] Set up a kubernetes cluster
- [ ] Deploy neo4j as a stateful set in its own namespace
- [ ] Set up mock kafka deployments which have ENV variables set to read and write 
to kafka topics, each with Services and Ingresses as well. Put one in its own namespace.
- [ ] Poll to import kubernetes resources as json / yaml via rest API into neo4j on a fixed interval
- [ ] Draw connections between the kubernetes deployments / pods
- [ ] Determine how / if I can cluster pods by their parent deployments
- [ ] Determine how / if I can cluster pods by their namespace
- [ ] Determine how / if I can connect services to pods
- [ ] Determine how / if I can connect ingresses to services
- [ ] Determine how / if I can connect Ingress Controller to all ingresses
- [ ] Determine how / if I can vary the size of the connection based on the amount of traffic
- [ ] Set up kafka for real
- [ ] Modify pods to (read / write) (from / to) (one / many) kafka topic(s)
- [ ] Determine how / if I can visualize kafka topic connections, varying size based on dataflow

## Setup

First get a kubernetes cluster going [like so](https://docs.docker.com/docker-for-mac/#kubernetes).

Pull this project from git. With a blank kubernetes cluster running, run the following from the project directory.

I enabled ingresses to avoid having to work with `port-forward` pods the entire time. This is optional, but I did 
it here so I recommend doing it as well to make it easier to follow along with the docs.

Enable ingresses, (as per 
[the offical docker-for-mac-ingress docs](https://github.com/kubernetes/ingress-nginx/blob/master/docs/deploy/index.md),
see for details)

```bash
# Mandatory command
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml

# Docker-for-mac command to create a Load Balancer service 
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/cloud-generic.yaml
```

Once this is done, apply the resources file to deploy the services specified in this repository

```
kubectl apply -f ./resources
```

This will load all kubernetes resources required to demonstrate neo4j and kubernetes integration.

Add the following to `/etc/hosts` to support ingresses as they've been configured

```
127.0.0.1  console.neo4j.croakerman.ca
127.0.0.1  backup.neo4j.croakerman.ca
127.0.0.1  database.neo4j.croakerman.ca
```

Navigate to `console.neo4j.croakerman.ca` and enter the following connection parameters when prompted:

| Connection Paramter | Value                                  |
| ------------------- | -------------------------------------- |
| server              | bolt://database.neo4j.croakerman.ca:80 |
| username            | neo4j                                  |
| password            | neo4j                                  |

Neo4j is now connected and it is possible to inspect the contents of the cluster.

## Gotchas

Current Gotchas list

* Running Neo4j in stand-alone mode. Would be pretty easy to convert to clustering mode though.
* Currently not configured for production; insecure.

## DEVLOG

The [DEVLOG](DEVLOG.md) provides insight into what and how I did. Key notes will be placed here.

## License

[MIT License 2.0](LICENSE)