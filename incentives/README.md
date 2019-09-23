# Swarm with SWAP enabled

This example will deploy a cluster with SWAP enabled and use Ganache to simulate an ethereum blockchain.

## Quick how to

Deploy or update a release:

```sh
# Check changes without applying, only show the diff
helmsman -f swarm.yaml --show-diff

# Apply changes
helmsman -f swarm.yaml --show-diff --apply
```

Delete a release:

```sh
# Using helmsman
helmsman -f swarm.yaml --destroy

# Using helm:

# List all releases
helm --tiller-namespace=$NAMESPACE list -a

# Destroy a release named 'swarm-private'
helm --tiller-namespace=$NAMESPACE delete swarm-private --purge

# Delete PVC's (volumes)
kubectl -n $NAMESPACE delete pvc -l app=swarm-private
```


## Useful tools

Access Grafana (Metrics):

```sh
kubectl -n rafael port-forward service/swarm-private-grafana 3001:80
# Then open http://localhost:3001
```

Access Jaeger (Tracing):

```sh
```

Access Kibana (Logs):

```sh
kubectl -n logging port-forward service/kibana 8443:443
# Then open http://localhost:8443

# You can also use kubectl to check the logs of any container
kubectl -n $NAMESPACE logs -f --tail=100 swarm-private-0 -c swarm
```

Interact with RPC via websockets:

```sh
kubectl proxy &
echo "bzz_kademliaInfo" | websocat ws://127.0.0.1:8001/api/v1/namespaces/$NAMESPACE/pods/http:swarm-private-0:8546/proxy/ --origin localhost --jsonrpc -n --one-message | jq '.'
```
