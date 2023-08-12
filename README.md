# minikube-harbor
minikube上でHarborを構築

## 参考
https://qiita.com/ysakashita/items/12e2f1e902ca5cbd56ed

## 環境
```
# uname -r
3.10.0-1160.el7.x86_64
```

```
# minikube version
minikube version: v1.21.0
commit: 76d74191d82c47883dc7e1319ef7cebd3e00ee11
```

NodeのIPは、10.1.1.200
```
# ifconfig 
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.1.1.200  netmask 255.255.255.0  broadcast 10.1.1.255
        inet6 fe80::6713:7817:6317:b77e  prefixlen 64  scopeid 0x20<link>
        ether 00:15:5d:01:10:43  txqueuelen 1000  (Ethernet)
        RX packets 977251  bytes 1192902700 (1.1 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 284034  bytes 75827329 (72.3 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

ingress を使用不可にしている
```
# minikube addons list 
|-----------------------------|----------|--------------|
|         ADDON NAME          | PROFILE  |    STATUS    |
|-----------------------------|----------|--------------|
| ambassador                  | minikube | disabled     |
| auto-pause                  | minikube | disabled     |
| csi-hostpath-driver         | minikube | disabled     |
| dashboard                   | minikube | disabled     |
| default-storageclass        | minikube | enabled ✅   |
| efk                         | minikube | disabled     |
| freshpod                    | minikube | disabled     |
| gcp-auth                    | minikube | disabled     |
| gvisor                      | minikube | disabled     |
| helm-tiller                 | minikube | disabled     |
| ingress                     | minikube | disabled     |
| ingress-dns                 | minikube | disabled     |
| istio                       | minikube | disabled     |
| istio-provisioner           | minikube | disabled     |
| kubevirt                    | minikube | disabled     |
| logviewer                   | minikube | disabled     |
| metallb                     | minikube | disabled     |
| metrics-server              | minikube | disabled     |
| nvidia-driver-installer     | minikube | disabled     |
| nvidia-gpu-device-plugin    | minikube | disabled     |
| olm                         | minikube | disabled     |
| pod-security-policy         | minikube | disabled     |
| registry                    | minikube | disabled     |
| registry-aliases            | minikube | disabled     |
| registry-creds              | minikube | disabled     |
| storage-provisioner         | minikube | enabled ✅   |
| storage-provisioner-gluster | minikube | disabled     |
| volumesnapshots             | minikube | disabled     |
|-----------------------------|----------|--------------|
```

## 手順

1. Ingress Controllerのセットアップ

    ```
    # kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.1/deploy/static/provider/cloud/deploy.yaml
    ```

1. KubernetesのNodeのIPをExternal IPとして利用します。ServiceのManifest(service-nodeport.yaml)をデプロイします。

    ```
    # kubectl apply -f service-nodeport.yaml
    ```

1. デプロイされたIngress Controllerを確認します。

    ```
    # kubectl get pod -n ingress-nginx
    ```

    ```
    # kubectl get svc -n ingress-nginx
    ```

1. HarborをデプロイするNamespaceを作成します。

    ```
    # kubectl create ns harbor
    ```


    ~~# kubens harbor~~ 
