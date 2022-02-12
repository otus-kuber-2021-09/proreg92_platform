## Installation MetalLB

1. kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.11.0/manifests/namespace.yaml
2. kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.11.0/manifests/metallb.yaml
3. kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"

Note: Need to edit /etc/resolv.conf --> add 'nameserver 8.8.8.8':
```sh
echo -e “nameserver 8.8.8.8\n$(cat /etc/resolv.conf)” > /etc/resolv.conf
```

## Checking readiness
```yaml
kubectl --namespace metallb-system get all
```

### Pre-requisites for IPVS

1. brew install hyperkit
2. minikube config set driver hyperkit
3. 'minikube delete' --> 'minikube start --extra-config="kubelet.allowed-unsafe-sysctls=net.*" --extra-config="kube-proxy.mode=ipvs"'
4. Checking diffs CM of kube-proxy:
    ```yaml
    kubectl get configmap kube-proxy -n kube-system -o yaml | \
      sed -e "s/strictARP: false/strictARP: true/" | \
    kubectl diff -f - -n kube-system
    ```
5. Applying changes:
    ```yaml
    kubectl get configmap kube-proxy -n kube-system -o yaml | \
      sed -e "s/strictARP: false/strictARP: true/" | \
      kubectl apply -f - -n kube-system
    ```
6. Delete pod kube-proxy and wait 10sec



