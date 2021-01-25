# desktop-deluxe

Install desktop-deluxe:
```bash
kubectl create deployment desktop-deluxe --image=kasmweb/desktop-deluxe:1.7.0
kubectl expose deployment desktop-deluxe --port=6901 --name=desktop-deluxe
```

Setup password:
```bash
kubectl exec -it deploy/desktop-deluxe -- bash

# run this inside container
echo "password" | vncpasswd -f > /home/kasm-user/.vnc/passwd
```

Setup ingress:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: desktop-deluxe
  annotations:
    nginx.org/websocket-services: desktop-deluxe
spec:
  tls:
    - hosts:
      - desktop-deluxe.k8s.shubhamtatvamasi.com
      secretName: letsencrypt
  rules:
    - host: desktop-deluxe.k8s.shubhamtatvamasi.com
      http:
        paths:
        - path: /
          backend:
            serviceName: desktop-deluxe
            servicePort: 6901
EOF
```

Delete everything:
```bash
kubectl delete deploy/desktop-deluxe svc/desktop-deluxe ing/desktop-deluxe
```
