
# Journey into Observability
> 13/10/2021: Codemotion Webinar - A tutto DevOps


## Install MiniKube

```
# link
https://minikube.sigs.k8s.io/docs/start/

# start
minikube start

# enable ingress
minikube addons enable ingress
```


## Install ArgoCD in k8s

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.1.2/manifests/install.yaml
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.1.2/manifests/install.yaml
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

- access ArgoCD UI
```
kubectl get svc -n argocd
kubectl port-forward svc/argocd-server 8080:443 -n argocd
```

- login with admin user and below token (as in documentation):
```
# from ubuntu shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo
```

- access at ArgoCD: http://localhost:8080
    - USR: admin
    - PWD: fE8BSKMOieyFHeYI


## Install Prometheus/Grafana stack

- https://computingforgeeks.com/setup-prometheus-and-grafana-on-kubernetes/

```
git clone https://github.com/prometheus-operator/kube-prometheus.git
cd kube-prometheus
kubectl create -f manifests/setup
kubectl create -f manifests/
```

- expose prometheus, grafana, alert manager

```
kubectl --namespace monitoring port-forward svc/grafana 3000
kubectl --namespace monitoring port-forward svc/prometheus-k8s 9090
kubectl --namespace monitoring port-forward svc/alertmanager-main 9093
```


## Deploy Aplication

In Argo, create an application with:
- name: guestbook
- url: https://github.com/FelipeObrero/journey.git
- path: kubernetes
- namespace: monitoring

## Access to Application

Exlore the ingress host via:

```
kubectl get ingress -n monitoring
```

You should see something like 

```
NAME                CLASS    HOSTS            ADDRESS        PORTS   AGE
guestbook-ingress   nginx    guestbook.info   192.168.49.2   80      10m
```

You need to add to your /etc/hosts a line

```
192.168.49.2    guestbook.info
```

After that you will reach:
- application -> http://guestbook.info
- api -> http://guestbook.info/api/hello
- prometheus metrics -> http://guestbook.info/metrics


### Notes:

I've installed ArgoCD on my kubernetes cluster using

```
> kubectl create namespace argocd
> kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

You can delete the entire installation using this:
```
> kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```


### Biblio:
- https://github.com/mastrogiovanni/codemotion-demo-2021.git
- https://gitlab.com/nanuchi/argocd-app-config
