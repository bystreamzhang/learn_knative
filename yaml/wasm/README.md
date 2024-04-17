make sure minikube using containerd, see<https://minikube.sigs.k8s.io/docs/handbook/config/>

```
$ kubectl apply -f runtime.yaml

$ kubectl run -it --rm --restart=Never wasi-demo --image=wasmedge/example-wasi:latest --annotations="module.wasm.image/variant=compat-smart" --overrides='{"kind":"Pod", "apiVersion":"v1", "spec": {"hostNetwork": true, "runtimeClassName": "crun"}}' /wasi_example_main.wasm 50000000

# set up knative serving
...


# open runtimeClass feature gate in Knative:

$ kubectl patch configmap/config-features -n knative-serving --type merge --patch '{"data":{"kubernetes.podspec-runtimeclassname":"enabled"}}'


# apply the serverless service configuration
# We need setup annotations, runtimeClassName, and ports.

$ kubectl apply -f http-wasm-serverless.yaml

# wait for a while, and check if the serverless service is available
$ kubectl get ksvc http-wasm

NAME          URL                                              LATESTCREATED       LATESTREADY         READY   REASON
http-wasm     http://http-wasm.default.knative.example.com     http-wasm-00001     http-wasm-00001

# Try to call the service
# As we do not set up DNS, we can only call the service via Kourier, Knative Serving ingress port.
# get Kourier port which is 31997 in following example
$ kubectl --namespace kourier-system get service kourier
NAME      TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)                      AGE
kourier   LoadBalancer   10.105.58.134                     80:31997/TCP,443:31019/TCP   53d
$ curl -H "Host: http-wasm.default.knative.example.com" -d "name=WasmEdge" -X POST http://localhost:31997

# check the new start pod
$ kubectl get pods
NAME                                           READY   STATUS    RESTARTS   AGE
http-wasm-00001-deployment-748bdc7cf-96l4r     2/2     Running   0          19s

```
