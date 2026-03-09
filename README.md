# grpc-on-gke
Companion repo for the Medium article gRPC on GKE for Fun &amp; Profit

### walkthrought

```
# deploy namespaces
kubectl apply -f namespaces/

# create dummy cert so grpcurl client can use TLS to talk to the managed load balancer
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key \
  -out tls.crt \
  -subj "/CN=example.com/O=MyOrganization"

kubectl -n gog-gateway create secret tls tls-cert-secret \
  --cert=tls.crt \
  --key=tls.key

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
grpcurl -insecure -proto whereami.proto $GATEWAY_IP:443 whereami.Whereami.GetPayload
```