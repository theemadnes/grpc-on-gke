# grpc-on-gke
Companion repo for the Medium article gRPC on GKE for Fun &amp; Profit

### walkthrought

```
# deploy namespaces
kubectl apply -f namespaces/

# deploy load balancer resources
kubectl apply -f gateway/

# deploy backend service
kubectl apply -n gog-backend -k backend/

# deploy frontend service
kubectl apply -n gog-frontend -k frontend/

# capture load balancer VIP
export GATEWAY_IP=$(kubectl get gateway external-http -n gog-gateway -o jsonpath='{.status.addresses[0].value}')
echo $GATEWAY_IP

# call the endpoint
grpcurl -plaintext -proto whereami.proto $GATEWAY_IP:80 whereami.Whereami.GetPayload
```