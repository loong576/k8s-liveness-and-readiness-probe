# k8s实践(五)：容器探针(liveness and readiness probe)
**环境说明：**

 
| 主机名 | 操作系统版本 | ip | docker version | kubelet version | 配置 | 备注 |
| :------: | :------:  | :------: | :------: | :------: | :------: |:------: |
| master | Centos 7.6.1810 | 172.27.9.131 |Docker 18.09.6 | V1.14.2 | 2C2G | 备注 |
| node01 | Centos 7.6.1810 | 172.27.9.135 |Docker 18.09.6 | V1.14.2 | 2C2G | 备注 |
| node02 | Centos 7.6.1810 | 172.27.9.136 |Docker 18.09.6 | V1.14.2 | 2C2G | 备注 |


<br>
&emsp; &emsp;只要将pod调度到某个节点，Kubelet就会运行pod的容器，如果该pod的容器有一个或者所有的都终止运行(容器的主进程崩溃)，Kubelet将重启容器，所以即使应用程序本身没有做任何特殊的事，在Kubemetes中运行也能自动获得自我修复的能力。

<br>

&emsp; &emsp; 自动重启容器以保证应用的正常运行，这是使用Kubernetes的优势，不过在某些情况，即使进程没有崩溃，有时应用程序运行也会出错。默认情况下Kubernetes只是检查Pod容器是否正常运行，但容器正常运行并不一定代表应用健康，在以下两种情况下Kubernetes将不会重启容器：

<br>

&emsp; &emsp; Kubemetes可以通过存活探针(liveness probe)检查容器是否还在运行。可以为pod中的每个容器单独指定存活探针。如果探测失败，Kubemetes将定期执行探针并重新启动容器。

<br>

&emsp; &emsp; ReadinessProbe用于容器的自定义准备状态检查。如果ReadinessProbe检查失败，Kubernetes会将该Pod从服务代理的分发后端去除，不再分发请求给该Pod。

<br>
<br>

**文章目录：**
# 一、为什么需要容器探针
##  如何保持Pod健康

# 二、LivenessProbe
## 1. 概念
## 2. exec探针
### 2.1 创建liveness-exec.yaml
### 2.2 查看Pod
## 3. HTTP探针
### 3.1 创建liveness-http.yaml
### 3.2 查看Pod
### 3.3 删除测试页面health
## 4. TCP探针
### 4.1 创建liveness-tcp.yaml
### 4.2 修改默认端口
### 4.3 查看Pod

# 三、ReadinessProbe
## 1. 概念
## 2. readinessprobe使用场景
## 3. 机制
## 4. 创建readiness-exec.yaml
## 5. 查看Pod
## 6. 与livenessprobe区别


<br>
<br>

**详细搭建过程及测试：**

https://blog.51cto.com/3241766/2431728
