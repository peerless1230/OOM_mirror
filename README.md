# OOM_mirror
Mirror of ONAP OOM project：[ONAP OOM](https://git.onap.org/oom)

## OOM项目简介
ONAP Operations Manager，设计目标是：负责ONAP平台的组件生命周期管理，目前大多使用OOM来完成ONAP整体系统在Kubernetes上的快速部署。此mirror项目的关注点也是ONAP与Kubernetes的集成

## release-1.0分支部署
```
curl http://10.20.23.240/softwares/docker/get_1.0_from_nexus.sh | sh
docker run -d –restart=unless-stopped -p 8880:8080 rancher/server
```
等待`rancher`初始化后，配置一个`Kubernetes`环境
### 添加k8s环境
__界面__ → __Default__ → __环境管理__ → __添加环境__ → __命名&&选择Kubernetes__ → __确认__ → __设置环境为缺省&&切换环境__
### 添加主机到集群
__基础架构__ → __添加主机__ → __按指示操作__
### Kubectl &&Helm 安装
Kubectl 版本1.7.4；helm版本2.3.0
```
wget -P /usr/local/bin/ http://10.20.23.240/softwares/docker/kubectl http://10.20.23.240/softwares/docker/helm
chmod +x /usr/local/bin/kubectl /usr/local/bin/helm
```
### 设置kubectl
__Rancher界面__ → __KUBERNETES__ → __CLI__ → __生成配置__

### 安装onap
```
## 下载oom 代码
git clone -b release-1.0.0 https://github.com/peerless1230/oom_mirror
source ~/oom/kubernetes/oneclick/setenv.bash
cd ~/oom/kubernetes/config
## 设定与openstack连接的参数
vi onap-parameters.yaml
./createConfig.sh -n onap  (等待pod处于Terminated: Completed)
cd ../oneclick
./createAll.bash -n onap
```
`release-1.0`的更多细节请查阅 [OOM部署ONAP](http://notafraid.me/2017/09/30/oom_deploy_onap/)

## master&Amsterdam版本分支部署
可利用社区的脚本，完成Rancher及其ONAP环境（即Rancher Kubernetes的改名）初始化
```
wget https://jira.onap.org/secure/attachment/11117/oom_rancher_setup.sh
# 以下操作，需等待15分钟左右
./oom_rancher_setup.sh -b master -s cd.onap.info -e onap
```
```
wget https://jira.onap.org/secure/attachment/11122/cd.sh
chmod 777 cd.sh
wget https://jira.onap.org/secure/attachment/11124/aaiapisimpledemoopenecomporg.cer
wget https://jira.onap.org/secure/attachment/11125/onap-parameters.yaml
wget https://jira.onap.org/secure/attachment/11126/aai-cloud-region-put.json

# 以下操作，等待时间较长，取决于pull docker镜像的速度
./cd.sh -b master
```
`master&Amsterdam`的更多细节请查阅 [ONAP Wiki - ONAP+on+Kubernetes](https://wiki.onap.org/display/DW/ONAP+on+Kubernetes)
