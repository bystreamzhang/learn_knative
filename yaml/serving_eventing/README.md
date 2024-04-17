refer to<https://knative.dev/docs/install/yaml-install/serving/install-serving-with-yaml/>

# serving:

```
kubectl apply -f serving-crds.yaml

kubectl apply -f serving-core.yaml

kubectl apply -f kourier.yaml

kubectl patch configmap/config-network \
  --namespace knative-serving \
  --type merge \
  --patch '{"data":{"ingress-class":"kourier.ingress.networking.knative.dev"}}'
  
kubectl --namespace kourier-system get service kourier
  
kubectl get pods -n knative-serving
  
kubectl apply -f serving-default-domain.yaml
```

# test serving:
```
kn service create hello \
--image ghcr.io/knative/helloworld-go:latest \
--port 8080 \
--env TARGET=World

kn service list

echo "Accessing URL $(kn service describe hello -o url)"
curl "$(kn service describe hello -o url)"

```

  
# eventing:
 
```
kubectl apply -f eventing-crds.yaml

kubectl apply -f eventing-core.yaml

kubectl get pods -n knative-eventing

//kubectl apply -f eventing-kafka-controller.yaml

//kubectl apply -f eventing-kafka-channel.yaml

kubectl apply -f in-memory-channel.yaml

kubectl apply -f mt-channel-broker.yaml

kubectl apply -f example-broker.yaml
```



