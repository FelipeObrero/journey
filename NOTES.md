
# ArgoCD

- Console: http://localhost:8080
    - USR: admin
    - PWD: KnMrsc3SrmOdhUkN

```
# host
192.168.49.2    guestbook.info
```

- reach application at http://guestbook.info
- API at http://guestbook.info/api/hello
- Backend prometheus metrics: http://guestbook.info/metrics

- reach application at http://192.168.49.2
- API at http://192.168.49.2/api/hello
- Backend prometheus metrics: http://192.168.49.2/metrics

```
kubectl get namespace

# inspect namespace 'ingress-nginx'
kubectl get all -n ingress-nginx
kubectl get pod -n ingress-nginx
kubectl get service -n ingress-nginx

# expose controller
kubectl --namespace ingress-nginx port-forward service/ingress-nginx-controller 8888:80
kubectl logs -n ingress-nginx pod/ingress-nginx-controller-6cc5ccb977-bn8gj --all-containers


# inspect namespace 'monitoring'
kubectl get all -n monitoring
kubectl get service -n monitoring
kubectl get ingress -n monitoring
---
NAME                CLASS   HOSTS            ADDRESS        PORTS   AGE
guestbook-ingress   nginx   guestbook.info   192.168.49.2   80      28m
---

# expose back-end
kubectl --namespace monitoring port-forward service/guestbook-backend 8891:80

# expose front-end
kubectl --namespace monitoring port-forward service/guestbook-ui 8892:80
```
