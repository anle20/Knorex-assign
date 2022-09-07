This is an in-memory key-value store HTTP API service, with the following endpoints:

- `/get/{key}` : GET method. Returns the value of a previously set key.

- `/set` : POST method. Sets key/value pair(s) in the key-value store. Body can accept multiple key-value pairs in a single request, for example - `{"abc-1":1,"abc-2":2,"xyz-1":"three","xyz-2":4}`.

- `/search` : GET method. Searches for keys using prefix or suffix filters.

Assume you have the following keys in the store: abc-1, abc-2, xyz-1, xyz-2.    
    - `/search?prefix=abc` would return `abc-1` and `abc-2`.    
    - `/search?suffix=-1` would return `abc-1` and `xyz-1`.

- `/` : GET method. Returns all the key-value pairs in the in-memory store. Also useful for readiness probes and load testing. 

- Access Link: http://knorex-lb-ca9d883fbe96f147.elb.ap-southeast-1.amazonaws.com/

### Infra Architecture

![knx-architect](https://user-images.githubusercontent.com/52650121/188896332-a32e2622-6f45-4241-8c2f-4a4b19e0204d.png)

### Technical stacks

- AWS EKS
- Helm chart
- AWS Network Load Blancer
- Circle CI
- Docker - Docker hub

### Known issues while running on Kubernetes

- The deployment creates 3 replicas of the key-value store service. These replicas do not have a shared memory, and so have different key-value pair stores, instead of all replicas utilizing a common store. We can implement database or using storage classes to handle this issue
- Key-value pairs are not stored anywhere outside the pod, thus, while requests can be made consistently without downtime, any stored values are lost when a pod restarts or goes down.

### Source code reference
https://github.com/importhuman/kv-store
