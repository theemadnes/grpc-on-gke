# grpc-on-gke
Companion repo for the Medium article gRPC on GKE for Fun &amp; Profit

### walkthrought

```
# deploy namespaces
kubectl apply -f namespaces/

# deploy load balancer resources
kubectl apply -f gateway/

# deploy backend service
kubectl apply -k backend/
```