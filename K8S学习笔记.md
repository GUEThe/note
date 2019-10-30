# K8S学习


## kubeadm

### 使用kubeadm来创建集群

``````
# 拉取所需镜像
kubeadm config images list
# 可能会报主机名错误	could not convert cfg to an internal cfg: name: Invalid value, 修改主机名再执行即可
``````

生成默认kubeadm.conf文件
``````
kubeadm config print init-defaults > kubeadm.conf
``````

编辑kubeadm.conf，将imageRepository修改为registry.aliyuncs.com/google_containers。并确认Kubernetes版本是和前文中的镜像列表的版本保持一致
``````
vim kubeadm.conf
``````

``````
# --kubernetes-version k8s版本，指定Kubenetes版本，如果不指定该参数，会从google网站下载最新的版本信息。
# --pod-network-cidr 指定pod网络的IP地址范围，它的值取决于你在下一步选择的哪个网络网络插件，比如我在本文中使用的是Calico网络，需要指定为192.168.0.0/16
# --apiserver-advertise-address 指定master服务发布的Ip地址，如果不指定，则会自动检测网络接口，通常是内网IP。
kubeadm init --kubernetes-version=v1.16.2 --pod-network-cidr=172.16.0.0/16 --apiserver-advertise-address=192.168.0.109
``````

init完成之后记录
``````
# 用于node加入master
kubeadm join 192.168.0.109:6443 --token 8j42ld.26x05sea5uipgcrn \
    --discovery-token-ca-cert-hash sha256:3a104ec7c9f1008c9eb8d5287b3bed6631fb72c97d1da411567a58090f198587
``````

执行init返回的命令

``````
# Your Kubernetes control-plane has initialized successfully!
# To start using your cluster, you need to run the following as a regular user:
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
``````

## kubectl

### 使用kubectl来管理Kubernetes集群。

可以在[kubernetes](https://github.com/kubernetes/kubernetes)找到更多的信息。

``````
# 查看版本
kubectl version

# 查看节点
kubectl get nodes
``````
在k8s运行你的第一个应用之前，你需要新建一个deployment。

deployment要有名称，并且提供你的应用的镜像地址。

We need to provide the deployment name and app image location (include the full repository url for images hosted outside Docker hub).

``````
# 创建deployment
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
``````
创建deployment之后将帮你执行以下操作

1、查找可以跑你的应用的适当的节点。

2、将你的应用在节点上运行起来。

3、配置集群以在需要时在新节点上重新安排实例。

``````
# 查看deployment
kubectl get deployments
``````

``````
# 启动代理
kubectl proxy
ctrl+c关闭
``````

注意主机名不能包含下划线
