* start minikube
* start [kubernetes dashboard](http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard:/proxy/#!/overview?namespace=_all)
* use kubernetes docker registry as default ```eval $(minikube docker-env)```
  * didn't work, pushed my fake-service image to my docker hub as alexsuzume/fake-service
* build it ```docker-compose build``` # not needed since we are pulling now an already build it image from docker hub
* create deploy yml with kompose ```kompose convert -f docker-compose.yml -o deploy.yml``
* deploy it! ```kubectl create -f deploy.yml```
* expose deploy ```kubectl expose deployment fake-server --name fake-server-lb --type=LoadBalancer --port 8080```
* now test it in ```kube service fake-server-lb --url```
* install [linkerd](https://linkerd.io/2/getting-started/)
* mesh services ```kubectl get -n default deploy -o yaml | linkerd inject - | kubectl apply -f -```
* see stream of requests ```linkerd -n default top deploy/fake-server```
* start linkerd dashboard ```linkerd dashboard```
  * linkerd dashboard an embedded graphana for each service
* enjoy!
