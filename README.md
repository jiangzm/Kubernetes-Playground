## Kubernetes Playground

### 预先拉取所需镜像

```bash
./load_images.sh
```

### 开启 Kubernetes

在`docker-desktop`->`Preferences`->`Kubernetes`开启Kubernetes

可选操作: 切换Kubernetes运行上下文至 docker-desktop (之前版本的 context 为 docker-for-desktop)


```shell
kubectl config use-context docker-desktop
```

验证 Kubernetes 集群状态

```shell
kubectl cluster-info
kubectl get nodes
```

### 安装 Kubernetes 控制台

> Kubernetes Dashboard 和 kubepi-server 选择其一即可

#### 部署 Kubernetes Dashboard
```shell
kubectl apply -f kubernetes-dashboard.yaml
```
通过如下 URL 访问 Kubernetes dashboard

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

开启 API Server 访问代理

```shell
kubectl proxy
```

#### 部署 kubepi-server和metrics-server

```shell
sudo docker run --privileged -d -v ~/kubepi:/var/lib/kubepi --restart=unless-stopped -p 800:80 --name kubepi kubeoperator/kubepi-server
```
```shell
kubectl apply -f metrics-server.yaml
```

通过如下 URL 访问 kubepi-server

http://localhost:800/

#### 配置服务账号

```shell
kubectl apply -f kube-system-admin.yaml
```

```shell
TOKEN=$(kubectl -n kube-system describe secret default| awk '$1=="token:"{print $2}')
kubectl config set-credentials docker-desktop --token="${TOKEN}"
echo $TOKEN
```

### 安装 Ingress Controller

```shell
kubectl apply -f ingress-nginx-controller.yaml
```

### 安装 cert-manager

```shell
kubectl apply -f cert-manager.yaml
```

### 部署 echo-server

```shell
kubectl apply -f echo-server.yaml
```