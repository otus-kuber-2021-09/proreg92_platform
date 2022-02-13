## Kubernetes Networks

### Create and work with Web App Resources/manifests
1. Web Deployment
2. Web Service: CIP, Headless, LB
3. Web Ingress
    ```bash
    $ curl http://172.17.255.2/web/index.html
    ```
4. Create Service ingress-nginx type LoadBalancer
5. Added liveness and readiness probes for Web Pod

### Installed, Worked with MetalLB, CoreDNS, IPVS
1. See MetalLB.md
2. Create CM for MetalLB
3. Added manifests for creating services for coredns: core/coredns-svc-lb-tcp, core/coredns-svc-lb-udp
4. Checking that IPVS works:
    ```bash
    $ k get svc -n default   
    NAME          TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)        AGE
    kubernetes    ClusterIP      10.96.0.1        <none>         443/TCP        3h51m
    web-svc       ClusterIP      None             <none>         80/TCP         160m
    web-svc-cip   ClusterIP      10.102.162.195   <none>         80/TCP         3h46m
    ```
    ```bash
    $ minikube ssh
    $ toolbox
    $ [root@minikube ~]# ipvsadm --list -n

    TCP  10.102.162.195:80 rr
      -> 172.17.0.3:8000              Masq    1      0          0
      -> 172.17.0.4:8000              Masq    1      0          1
      -> 172.17.0.5:8000              Masq    1      0          1

    $ curl http://10.102.162.195/index.html

    $ curl -i http://10.102.162.195/index.html
      HTTP/1.0 200 OK
      Server: SimpleHTTP/0.6 Python/3.10.0
      Date: Tue, 02 Nov 2021 16:56:30 GMT
      Content-type: text/html
      Content-Length: 83851
      Last-Modified: Tue, 02 Nov 2021 13:00:22 GMT

    <html>
    <head/>
    <body>
    <!-- IMAGE BEGINS HERE -->
    <font size="-3">
    ```

#### CoreDNS checking via MetalLB:
1. My IP-addresses:
    ```bash
    NAME          TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)        AGE
    kubernetes    ClusterIP      10.96.0.1        <none>         443/TCP        53m
    web-svc-cip   ClusterIP      10.102.162.195   <none>         80/TCP         48m
    web-svc-lb    LoadBalancer   10.108.184.11    172.17.255.1   80:32379/TCP   41m
    ```
2. minikube ssh: (не стал пробросы делать на моей ОС)
    ```bash
    $ nslookup web-svc-lb.default.svc.cluster.local 172.17.255.10
    Server:		172.17.255.10
    Address:	172.17.255.10:53

    *** Can't find web-svc-lb.default.svc.cluster.local: No answer

    Name:	web-svc-lb.default.svc.cluster.local
    Address: 10.108.184.11
    ```


### K8S Dashboard and Ingress: 

1. Installation
    ```yaml
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
    ```

2. After installation we already have kubernetes-dashboard service in namespace 'kubernetes-dashboard'.
Then we just create additional ingress for kubernetes-dashboard (dashboard/<manifests.yaml>)

3. Checking:
    ```bash
    $ curl -i http://172.17.255.2/dashboard
        HTTP/1.1 200 OK
        Date: Tue, 02 Nov 2021 15:03:13 GMT
        Content-Type: text/html; charset=utf-8
        Content-Length: 1338
        Connection: keep-alive
        Accept-Ranges: bytes
        Cache-Control: no-cache, no-store, must-revalidate
        Last-Modified: Fri, 15 Oct 2021 07:41:12 GMT
    ```

### Canary deployments via Ingress

1. Create web-prod.yaml manifest with 2 replicas of web-app pod in 'web-prod' namespace

    ```bash 
    $ k describe ingress web -n web-prod                                                        
      Name:             web
      Namespace:        web-prod
      Address:          192.168.64.13
      Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
      Rules:
        Host        Path  Backends
        ----        ----  --------
        *
                    /web   web-svc:8000 (172.17.0.3:8000,172.17.0.4:8000)
      Annotations:  kubernetes.io/ingress.class: nginx
                    nginx.ingress.kubernetes.io/rewrite-target: /
      Events:
        Type    Reason  Age                    From                      Message
        ----    ------  ----                   ----                      -------
        Normal  Sync    2m51s (x2 over 2m54s)  nginx-ingress-controller  Scheduled for sync
    ```

2. Create web-canary.yaml manifest with 1 replica of web-app pod in 'web-canary' namespace

    ```bash
    $ k describe ingress web -n web-canary 
      Name:             web
      Namespace:        web-canary
      Address:          192.168.64.13
      Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
      Rules:
        Host        Path  Backends
        ----        ----  --------
        *
                    /web   web-svc:8000 (172.17.0.8:8000)
      Annotations:  kubernetes.io/ingress.class: nginx
                    nginx.ingress.kubernetes.io/canary: true
                    nginx.ingress.kubernetes.io/canary-weight: 30
                    nginx.ingress.kubernetes.io/rewrite-target: /
      Events:
        Type    Reason  Age                   From                      Message
        ----    ------  ----                  ----                      -------
        Normal  Sync    105s (x2 over 2m22s)  nginx-ingress-controller  Scheduled for sync
    ```

