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

## Step 1: Set up a kubernetes cluster

Using [kubernetes cluster built into docker](https://www.docker.com/products/kubernetes). To enable, download docker
and turn on the kubernetes cluster.

## Step 2: Deploy neo4j as a stateful set in its own namespace

