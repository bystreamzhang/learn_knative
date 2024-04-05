

// cat serving-core.yaml | grep 'registry.cn-hangzhou.aliyuncs.com' | awk '{print $2}' | sort | uniq | xargs -n 1 cosign verify -o text --certificate-identity=signer@knative-releases.iam.gserviceaccount.com --certificate-oidc-issuer=https://accounts.google.com


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
