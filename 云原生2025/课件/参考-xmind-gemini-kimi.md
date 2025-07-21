# Kubernetes Patterns 16 周 PBL 课程思维导图

## ✅ 使用方式
1. 打开 XMind → 文件 → 导入 → Markdown
2. 粘贴本文件内容 → 自动生成思维导图
3. 展开/折叠节点即可浏览每周内容

---

## 📅 Week 1：从单体到云原生（讲授）
### 🎯 核心问题
- 为什么现代应用不再跑在单台服务器？

### 📊 Slide 1：课程开场
- 图：图 1-1（PDF p2）
- 内容：单体 → 微服务 → 云原生

### 📊 Slide 2：云原生四大挑战
- 可移植性、可扩展性、可观测性、可恢复性

### 📊 Slide 3：K8s 是分布式操作系统
- 表 1-1（PDF p4）：Java vs K8s 原语对比

### 📊 Slide 4：Java vs K8s 类比
- 类 → Pod，JVM → 节点，包 → 命名空间

### 📊 Slide 5：本周小结
- 预告 Week 2：如何“装”应用进 Pod？

---

## 📅 Week 2：容器与 Pod 初体验（实验）
### 🎯 实验目标
- 运行第一个 Pod，理解生命周期

### 🧪 Slide 1：启动单节点集群
- 命令：`kind create cluster` / `minikube start`

### 🧪 Slide 2：运行 Nginx Pod
- 命令：`kubectl run nginx --image=nginx`

### 🧪 Slide 3：观察 Pod 状态
- 命令：`kubectl get pods -o wide`

### 🧪 Slide 4：进入容器
- 命令：`kubectl exec -it nginx -- /bin/bash`

### 🧪 Slide 5：清理资源
- 命令：`kubectl delete pod nginx`

---

## 📅 Week 3：如何描述一个“应用”在 K8s 中的存在（讲授）
### 🎯 核心问题
- K8s 没有“Application”，如何拼出应用？

### 📊 Slide 1：K8s 开发者资源全景
- 图 1-4（PDF p11）

### 📊 Slide 2：标签与选择器
- 图 1-3（PDF p8）

### 📊 Slide 3：命名空间隔离
- 命令：`kubectl create ns dev`

### 📊 Slide 4：资源配额
- YAML：ResourceQuota（PDF p23）

### 📊 Slide 5：本周小结
- 预告 Week 4：如何防止资源争抢？

---

## 📅 Week 4：创建并约束你的第一个应用（实验）
### 🎯 实验目标
- 在 dev/prod 命名空间中部署带资源限制的 Pod

### 🧪 Slide 1：创建命名空间
```bash
kubectl create ns dev
kubectl create ns prod
```

### 🧪 Slide 2：部署无资源限制的 Pod
- YAML：无 resources 字段

### 🧪 Slide 3：部署带资源限制的 Pod
- YAML：requests/limits

### 🧪 Slide 4：观察 QoS 等级
- 命令：`kubectl get pod -o yaml | grep qosClass`

### 🧪 Slide 5：验证驱逐行为
- 命令：`kubectl describe node`

---

## 📅 Week 5：如何让应用“活着”并“优雅地死”（讲授）
### 🎯 核心问题
- 如何知道 Pod 健康？如何优雅退出？

### 📊 Slide 1：健康探针全景
- 图 4-1（PDF p48）

### 📊 Slide 2：LivenessProbe
- 代码：HTTP /health（PDF p43）

### 📊 Slide 3：ReadinessProbe
- 代码：exec 检查文件（PDF p44）

### 📊 Slide 4：StartupProbe
- 代码：WildFly 启动探针（PDF p46）

### 📊 Slide 5：优雅停机机制
- 代码：preStop sleep（PDF p54）

---

## 📅 Week 6：打造“打不死”的应用（实验）
### 🧪 Slide 1：部署完整健康探针
- YAML：liveness + readiness + startup

### 🧪 Slide 2：模拟死锁
```bash
kubectl exec -it <pod> -- curl /block
```

### 🧪 Slide 3：观察重启
```bash
kubectl get pods -w
```

### 🧪 Slide 4：配置优雅停机
- YAML：preStop hook

### 🧪 Slide 5：验证 30s 优雅期
```bash
kubectl delete pod <pod> --grace-period=30
```

---

## 📅 Week 7：如何自动化部署与回滚（讲授）
### 🎯 核心问题
- 如何发布 v1.1 不中断服务？

### 📊 Slide 1：RollingUpdate 图
- 图 3-1（PDF p32）

### 📊 Slide 2：Deployment 结构
- 图 3-2（PDF p30）

### 📊 Slide 3：RollingUpdate 参数
- YAML：maxSurge/maxUnavailable（PDF p31）

### 📊 Slide 4：蓝绿部署
- 图 3-3（PDF p34）

### 📊 Slide 5：金丝雀发布
- 图 3-4（PDF p35）

---

## 📅 Week 8：第一次集群部署实践（实验）
### 🧪 Slide 1：实验目标
- 使用 kubeadm 部署 3 节点集群

### 🧪 Slide 2：初始化 Master
```bash
kubeadm init --pod-network-cidr=10.244.0.0/16
```

### 🧪 Slide 3：安装 Calico
```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml
```

### 🧪 Slide 4：加入 Worker 节点
```bash
kubeadm join <master-ip>:6443 --token <token>
```

### 🧪 Slide 5：验证集群
```bash
kubectl get nodes
```

---

## 📅 Week 9：如何让 Pod“物以类聚”（讲授）
### 🎯 核心问题
- 如何调度到 SSD/GPU 节点？如何避免同节点？

### 📊 Slide 1：Pod 调度图
- 图 6-3（PDF p73）

### 📊 Slide 2：NodeSelector
- 代码（PDF p65）

### 📊 Slide 3：Node Affinity
- 代码（PDF p66）

### 📊 Slide 4：Pod Anti-Affinity
- 代码（PDF p67）

### 📊 Slide 5：Taints & Tolerations
- 代码（PDF p70）

---

## 📅 Week 10：精准控制 Pod 部署位置（实验）
### 🧪 Slide 1：节点打标签
```bash
kubectl label nodes node-1 disktype=ssd
```

### 🧪 Slide 2：反亲和性部署
- YAML：PodAntiAffinity

### 🧪 Slide 3：验证调度结果
```bash
kubectl get pods -o wide
```

### 🧪 Slide 4：污点节点
```bash
kubectl taint nodes node-3 gpu=true:NoSchedule
```

### 🧪 Slide 5：容忍污点部署
- YAML：tolerations

---

## 📅 Week 11：如何构建可扩展无状态服务（讲授）
### 🎯 核心问题
- 如何部署可横向扩展的 Web 服务？

### 📊 Slide 1：无状态架构图
- 图 11-1（PDF p113）

### 📊 Slide 2：Deployment 结构
- 代码（PDF p108）

### 📊 Slide 3：Service Discovery
- 代码（PDF p110）

### 📊 Slide 4：水平扩缩容
```bash
kubectl scale deployment web --replicas=5
```

### 📊 Slide 5：无状态最佳实践
- 不保存状态，使用外部存储

---

## 📅 Week 12：部署并连接无状态微服务（实验）
### 🧪 Slide 1：部署 3 副本 Deployment
```yaml
replicas: 3
```

### 🧪 Slide 2：创建 ClusterIP Service
```yaml
selector:
  app: web
```

### 🧪 Slide 3：客户端访问
```bash
kubectl run client --image=curlimages/curl --rm -it -- curl http://web-service
```

### 🧪 Slide 4：扩容到 5 副本
```bash
kubectl scale deployment web --replicas=5
```

### 🧪 Slide 5：观察负载均衡
```bash
kubectl logs -l app=web
```

---

## 📅 Week 13：如何运行离线任务与定时任务（讲授）
### 🎯 核心问题
- 如何每天凌晨跑任务？

### 📊 Slide 1：Job 流程图
- 图 7-1（PDF p81）

### 📊 Slide 2：Job 基本结构
- 代码（PDF p80）

### 📊 Slide 3：CronJob 结构
- 代码（PDF p88）

### 📊 Slide 4：任务清理策略
- YAML：ttlSecondsAfterFinished

### 📊 Slide 5：失败重试策略
- restartPolicy: OnFailure vs Never

---

## 📅 Week 14：运行离线任务与暴露服务（实验）
### 🧪 Slide 1：创建 Job 打印斐波那契
- YAML：Job

### 🧪 Slide 2：创建 CronJob 每分钟打印时间
- YAML：CronJob

### 🧪 Slide 3：部署 Web 应用
- YAML：Deployment

### 🧪 Slide 4：创建 Ingress
- YAML：Ingress

### 🧪 Slide 5：访问测试
```bash
curl http://your-domain.com/app1
```

---

## 📅 Week 15：如何驾驭有状态的世界（讲授）
### 🎯 核心问题
- 如何部署 MySQL 集群？

### 📊 Slide 1：StatefulSet 架构图
- 图 12-1（PDF p120）

### 📊 Slide 2：StatefulSet 三大保证
- 稳定身份、独立存储、有序部署

### 📊 Slide 3：Headless Service
- 代码（PDF p120）

### 📊 Slide 4：volumeClaimTemplates
- 代码（PDF p119）

### 📊 Slide 5：Pod 命名规则
- mysql-0, mysql-1, mysql-2

---

## 📅 Week 16：综合项目实战（实验）
### 🧪 Slide 1：项目结构
- 前端 + 后端 + MySQL

### 🧪 Slide 2：部署 MySQL StatefulSet
- YAML：StatefulSet + PVC

### 🧪 Slide 3：部署后端 Deployment
- YAML：Deployment + Service

### 🧪 Slide 4：创建 Ingress 暴露前端
- YAML：Ingress

### 🧪 Slide 5：部署 CronJob 备份数据库
- YAML：CronJob

---
