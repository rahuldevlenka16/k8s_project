Ref : https://kind.sigs.k8s.io/docs/user/ingress/

apply nginx ingress controller :
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml


check the controller: 
kubectl -n ingress-nginx get services 
E.g. output: 
NAME                                 TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.111.75.61    localhost     80:30948/TCP,443:32724/TCP   12m
ingress-nginx-controller-admission   ClusterIP      10.103.96.146   <none>        443/TCP                      12m


apply the ingress resource yaml
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/usage.yaml


check the resources in demo-ingress ns:
kubectl get all -n demo-ingress

e.g. output :
NAME                           READY   STATUS    RESTARTS   AGE
pod/bar-app-5976669957-6fsg9   1/1     Running   0          6s
pod/bar-app-5976669957-985fj   1/1     Running   0          62s
pod/foo-app-58dc64cbd5-flbns   1/1     Running   0          62s
pod/foo-app-58dc64cbd5-xqbzl   1/1     Running   0          6s
NAME                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/bar-service   ClusterIP   10.102.12.160   <none>        8080/TCP   62s
service/foo-service   ClusterIP   10.109.202.3    <none>        8080/TCP   62s
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/bar-app   3/3     3            3           62s
deployment.apps/foo-app   3/3     3            3           62s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/bar-app-5976669957   3         3         3       62s
replicaset.apps/foo-app-58dc64cbd5   3         3         3       62s


to access the service use,
kubectl port-forward svc/foo-service 8088:8080 -n demo-ingress
kubectl port-forward svc/bar-service 8088:8080 -n demo-ingress


Try with curl,

curl localhost:80/foo
e.g. output:
D:\k8s project\ingress>curl localhost:80/foo
foo-app-58dc64cbd5-v66js
D:\k8s project\ingress>curl localhost:80/foo
foo-app-58dc64cbd5-xqbzl
D:\k8s project\ingress>curl localhost:80/foo
foo-app-58dc64cbd5-flbns

curl localhost:80/bar


