##traffic shifting
kubectl apply -f green-deployment.yaml -n traffic shifting
kubectl apply -f red-deployment.yaml -n traffic shifting

##intall istio
export ISTIO_VERSION=1.17.5
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=${ISTIO_VERSION} sh -

##label ejection
kubectl get ns --show-labels=true
kubectl label namespace traffic-shifting istio-injection=enabled

##verification
i=2; while [ $i -le 10 ]; do curl -H "Host: red-green-service.tahs.local" http://172.18.255.150:80; (( i++ )); sleep 1 ; done
while true; do curl -H "Host: red-green-service.tahs.local" http://172.18.255.150:80; sleep 1 ; done