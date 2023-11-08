
# ArgoCD
USR: admin
PWD: 4elxoZJbylAsQpx2

192.168.49.2 guestbook.info


- reach application at http://guestbook.info
- API at http://guestbook.info/api/hello
- Backend prometheus metrics: http://guestbook.info/metrics


- reach application at http://192.168.49.2
- API at http://192.168.49.2/api/hello
- Backend prometheus metrics: http://192.168.49.2/metrics

kubectl get namespace

# search ingress
kubectl get all -n ingress-nginx
kubectl get ingress -n ingress-nginx

# expose controller
kubectl --namespace ingress-nginx port-forward service/ingress-nginx-controller 8888:80
kubectl logs -n ingress-nginx pod/ingress-nginx-controller-6cc5ccb977-jrh4n --all-containers


# search ingress
kubectl get all -n monitoring
kubectl get ingress -n monitoring
    guestbook-ingress   nginx   guestbook.info   192.168.49.2   80      52m

# expose back-end
kubectl --namespace monitoring port-forward service/guestbook-backend 8891:80

# expose front-end
kubectl --namespace monitoring port-forward service/guestbook-ui 8892:80
