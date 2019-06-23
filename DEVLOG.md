# Devlog

What and how I did

## Step 1: Set up a kubernetes cluster

Using [kubernetes cluster built into docker](https://www.docker.com/products/kubernetes). To enable, download docker
and turn on the kubernetes cluster.

## Step 2: Deploy neo4j as a stateful set in its own namespace

I found deployment files for neo4j [here](https://github.com/neo4j-contrib/kubernetes-neo4j).

I added a namespace to the statefulset to make it a bit easier to separate out from the rest of the resources.

I followed [these steps](https://github.com/kubernetes/ingress-nginx/blob/master/docs/deploy/index.md) to set up ingress
for docker-for-mac. Specifically, the `all-deployments` command and the `docker-for-mac` command.

I [disabled casual clustering on neo4j](https://neo4j.com/docs/operations-manual/current/docker/configuration/) since 
I don't care about clustering and want to focus on visualization for now. See docs linked in order to set up core 
clustering.

```yaml
env:
  - name: NEO4J_dbms_mode
    value: SINGLE
```

I set up some ingresses and added them to my hosts file for fanciness factor.

```
127.0.0.1  console.neo4j.croakerman.ca
127.0.0.1  backup.neo4j.croakerman.ca
127.0.0.1  database.neo4j.croakerman.ca
```

&& added ingress

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: neo4j
  namespace: neo4j-ns
  labels:
    app: neo4j
    component: core
spec:
  rules:
  - host: console.neo4j.croakerman.ca
    http:
      paths:
      - path: /
        backend:
          serviceName: neo4j
          servicePort: 7474
  - host: backup.neo4j.croakerman.ca
    http:
      paths:
      - path: /
        backend:
          serviceName: neo4j
          servicePort: 6362
  - host: database.neo4j.croakerman.ca
    http:
      paths:
      - path: /
        backend:
          serviceName: neo4j
          servicePort: 7687
```

My connection string in the browser view took a while to figure out; I had to explicitly request port 80 of the 
database URI. My connection parameters were:

| Connection Paramter | Value                                  |
| ------------------- | -------------------------------------- |
| server              | bolt://database.neo4j.croakerman.ca:80 |
| username            | neo4j                                  |
| password            | neo4j                                  |

I guess at this point I would consider neo4j set-up.