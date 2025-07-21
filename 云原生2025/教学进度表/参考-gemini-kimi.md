以下是为《Kubernetes Patterns》16 周 PBL 课程（讲授+实验）重新设计的 **每周围绕 5 张 slide 的详细内容**，每张 slide 均注明了 **PDF 中的图片来源（章节+页码+图编号）** 与 **代码来源**，严格对应原书内容，确保图文一致、可追溯。

---

## 📅 Week 1：讲授 - 从单体到云原生（PDF Chapter 1）

### Slide 1：课程开场 & 问题设定
- **内容**：为什么现代应用不再运行在单台服务器？
- **图**：图 1-1 “The path to cloud native”（PDF Chapter 1, Page 2）
- **提示**：展示单体 vs 微服务 vs 云原生的演进路径

### Slide 2：云原生的四大挑战
- **内容**：可移植性、可扩展性、可观测性、可恢复性
- **图**：图 1-1 扩展路径图（PDF Chapter 1, Page 2）

### Slide 3：Kubernetes 是分布式操作系统
- **内容**：K8s 提供分布式原语（Pod、Service、Deployment）
- **图**：表 1-1 “Local vs Distributed Primitives”（PDF Chapter 1, Page 4）

### Slide 4：从 Java 到 K8s 的类比
- **内容**：类比 JVM 与 K8s 的运行时差异
- **图**：表 1-1 对比图（PDF Chapter 1, Page 4）

### Slide 5：本周小结 & 引出 Week 2
- **内容**：下一步：如何将应用“装”进 Pod？
- **提示**：预告容器与 Pod 的关系

---

## 📅 Week 2：实验 - 容器与 Pod 初体验

### Slide 1：实验目标
- **内容**：运行第一个 Pod，理解 Pod 生命周期
- **代码**：`kubectl run nginx --image=nginx`

### Slide 2：启动单节点集群
- **内容**：使用 kind/minikube
- **图**：无图，建议用终端截图

### Slide 3：观察 Pod 状态
- **内容**：使用 `kubectl get pods`, `describe`, `logs`
- **图**：Pod 状态图（PDF Chapter 1, Page 6）

### Slide 4：进入容器
- **内容**：`kubectl exec -it <pod> -- /bin/bash`
- **图**：Pod 结构图（PDF Chapter 1, Page 6 图 1-2）

### Slide 5：清理资源
- **内容**：`kubectl delete pod nginx`
- **提示**：为下周命名空间实验做准备

---

## 📅 Week 3：讲授 - 如何描述一个“应用”在 K8s 中的存在

### Slide 1：问题引入
- **内容**：K8s 中没有“应用”概念，如何用资源拼出应用？
- **图**：图 1-4 “Kubernetes concepts for developers”（PDF Chapter 1, Page 11）

### Slide 2：标签与选择器
- **内容**：如何用标签组织服务
- **图**：图 1-3 “Labels as application identity”（PDF Chapter 1, Page 8）

### Slide 3：命名空间隔离
- **内容**：dev/test/prod 隔离
- **代码**：`kubectl create ns dev`

### Slide 4：资源配额
- **内容**：防止资源滥用
- **代码**：ResourceQuota 示例（PDF Chapter 2, Page 23）

### Slide 5：本周小结
- **内容**：预告 Week 4：如何防止 Pod 抢资源？

---

## 📅 Week 4：实验 - 创建并约束你的第一个应用

### Slide 1：实验目标
- **内容**：部署 Nginx，设置资源限制
- **代码**：YAML Deployment 示例

### Slide 2：创建命名空间
- **代码**：
```bash
kubectl create ns dev
kubectl create ns prod
```

### Slide 3：部署无资源限制的 Pod
- **代码**：YAML 无 resources 字段
- **图**：Pod QoS 图（PDF Chapter 2, Page 20）

### Slide 4：部署带资源限制的 Pod
- **代码**：
```yaml
resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    memory: 256Mi
```

### Slide 5：验证 QoS 等级
- **命令**：`kubectl get pod <pod> -o yaml | grep qosClass`
- **图**：表 2-1 QoS 类型（PDF Chapter 2, Page 21）

---

## 📅 Week 5：讲授 - 如何让应用“活着”并“优雅地死”

### Slide 1：问题引入
- **内容**：Pod 状态为 Running 但无法响应请求怎么办？
- **图**：图 4-1 “Container observability options”（PDF Chapter 4, Page 48）

### Slide 2：LivenessProbe
- **代码**：HTTP /health 示例（PDF Chapter 4, Page 43）
- **图**：图 4-2 LivenessProbe 流程图

### Slide 3：ReadinessProbe
- **代码**：检查文件存在的 exec 示例（PDF Chapter 4, Page 44）
- **图**：图 4-3 ReadinessProbe 流程图

### Slide 4：StartupProbe
- **代码**：WildFly 启动探针（PDF Chapter 4, Page 46）
- **图**：图 4-4 StartupProbe 示例

### Slide 5：优雅停机机制
- **内容**：SIGTERM + preStop hook
- **代码**：preStop sleep 30s（PDF Chapter 5, Page 54）

---

## 📅 Week 6：实验 - 打造一个“打不死”的应用

### Slide 1：实验目标
- **内容**：配置完整健康探针

### Slide 2：部署带探针的 Web 应用
- **YAML**：liveness + readiness + startup

### Slide 3：模拟死锁
- **命令**：
```bash
kubectl exec -it <pod> -- curl /block
```

### Slide 4：观察重启行为
- **命令**：
```bash
kubectl get pods -w
```

### Slide 5：配置优雅停机
- **YAML**：
```yaml
preStop:
  exec:
    command: ["/bin/sh", "-c", "sleep 30"]
```

---

## 📅 Week 7：讲授 - 如何自动化部署与回滚

### Slide 1：问题引入
- **内容**：如何发布 v1.1 不中断服务？
- **图**：图 3-1 RollingUpdate 流程图（PDF Chapter 3, Page 32）

### Slide 2：Deployment 结构
- **图**：图 3-2 Deployment → ReplicaSet → Pod（PDF Chapter 3, Page 30）

### Slide 3：RollingUpdate 参数
- **代码**：maxSurge / maxUnavailable 示例（PDF Chapter 3, Page 31）

### Slide 4：蓝绿部署
- **图**：图 3-3 Blue-Green 流程图（PDF Chapter 3, Page 34）
- **代码**：Service selector 切换

### Slide 5：金丝雀发布
- **图**：图 3-4 Canary 流程图（PDF Chapter 3, Page 35）
- **代码**：两个 Deployment + 一个 Service

---

## 📅 Week 8：实验 - 第一次集群部署实践（新增）

### Slide 1：实验目标
- **内容**：使用 kubeadm 部署 3 节点集群

### Slide 2：初始化 Master
- **命令**：
```bash
kubeadm init --pod-network-cidr=10.244.0.0/16
```

### Slide 3：安装 Calico
- **命令**：
```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml
```

### Slide 4：加入 Worker 节点
- **命令**：
```bash
kubeadm join <master-ip>:6443 --token <token>
```

### Slide 5：验证节点
- **命令**：
```bash
kubectl get nodes
```


## 📅 Week 9：讲授 - 如何让 Pod“物以类聚”？（PDF Chapter 6）

### Slide 1：问题引入
- **问题**：如何确保某些 Pod 必须落在 SSD/GPU 节点？
- **图**：图 6-3 “Pod-to-Pod and Pod-to-Node dependencies”（PDF Chapter 6, Page 73）

### Slide 2：NodeSelector
- **内容**：最简单的节点硬约束
- **代码**（PDF Chapter 6, Page 65）：
```yaml
nodeSelector:
  disktype: ssd
```

### Slide 3：Node Affinity
- **内容**：required vs preferred
- **代码**（PDF Chapter 6, Page 66）：
```yaml
nodeAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
    nodeSelectorTerms:
    - matchExpressions:
      - key: numberCores
        operator: Gt
        values: ["3"]
```

### Slide 4：Pod Affinity & Anti-Affinity
- **内容**：避免副本落在同一节点
- **代码**（PDF Chapter 6, Page 67）：
```yaml
podAntiAffinity:
  requiredDuringSchedulingIgnoredDuringExecution:
  - labelSelector:
      matchLabels:
        app: web
    topologyKey: kubernetes.io/hostname
```

### Slide 5：Taints & Tolerations
- **内容**：节点排斥机制
- **命令**（PDF Chapter 6, Page 70）：
```bash
kubectl taint nodes node-1 special=true:NoSchedule
```
- **代码**（Pod 容忍）：
```yaml
tolerations:
- key: "special"
  operator: "Equal"
  value: "true"
  effect: "NoSchedule"
```

---

## 📅 Week 10：实验 - 精准控制 Pod 的部署位置（PDF Chapter 6）

### Slide 1：实验目标
- **问题**：如何让 2 个 Web 副本永远不在同一节点？

### Slide 2：节点打标签
```bash
kubectl label nodes node-1 disktype=ssd
kubectl label nodes node-2 disktype=hdd
```

### Slide 3：部署带反亲和性的 Pod
```yaml
affinity:
  podAntiAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
    - labelSelector:
        matchLabels:
          app: web
      topologyKey: kubernetes.io/hostname
```

### Slide 4：验证调度结果
```bash
kubectl get pods -o wide
```

### Slide 5：污点节点 & 容忍
```bash
kubectl taint nodes node-3 gpu=true:NoSchedule
```
```yaml
tolerations:
- key: gpu
  operator: Exists
  effect: NoSchedule
```

---

## 📅 Week 11：讲授 - 如何构建可扩展的无状态服务（PDF Chapter 11）

### Slide 1：问题引入
- **问题**：如何部署一个可横向扩展的 Web 服务？
- **图**：图 11-1 “Distributed stateless application on Kubernetes”（PDF Chapter 11, Page 113）

### Slide 2：Deployment 与 ReplicaSet
- **内容**：Deployment 管理副本
- **代码**（PDF Chapter 11, Page 108）：
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
```

### Slide 3：Service Discovery
- **内容**：ClusterIP + DNS
- **代码**（PDF Chapter 11, Page 110）：
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 8080
```

### Slide 4：水平扩缩容
```bash
kubectl scale deployment web --replicas=5
```

### Slide 5：无状态最佳实践
- **内容**：不保存状态，使用外部存储
- **图**：图 11-1 架构图（PDF Chapter 11, Page 113）

---

## 📅 Week 12：实验 - 部署并连接无状态微服务（PDF Chapter 11）

### Slide 1：实验目标
- **问题**：如何让多个副本共享一个入口？

### Slide 2：部署 3 副本 Deployment
```yaml
replicas: 3
```

### Slide 3：创建 ClusterIP Service
```yaml
selector:
  app: web
```

### Slide 4：从客户端 Pod 访问
```bash
kubectl run client --image=curlimages/curl --rm -it --restart=Never -- curl http://web-service
```

### Slide 5：观察负载均衡
```bash
kubectl logs -l app=web
```

---

## 📅 Week 13：讲授 - 如何运行离线任务与定时任务（PDF Chapter 7 + 8）

### Slide 1：问题引入
- **问题**：如何每天凌晨跑一个数据处理任务？
- **图**：图 7-1 “Parallel Batch Job”（PDF Chapter 7, Page 81）

### Slide 2：Job 基本结构
- **代码**（PDF Chapter 7, Page 80）：
```yaml
apiVersion: batch/v1
kind: Job
spec:
  completions: 5
  parallelism: 2
```

### Slide 3：CronJob 定时任务
- **代码**（PDF Chapter 8, Page 88）：
```yaml
apiVersion: batch/v1
kind: CronJob
spec:
  schedule: "0 2 * * *"
```

### Slide 4：任务清理策略
- **代码**：
```yaml
ttlSecondsAfterFinished: 300
```

### Slide 5：失败重试策略
- **内容**：restartPolicy: OnFailure vs Never

---

## 📅 Week 14：实验 - 运行离线任务与暴露 Web 服务（PDF Chapter 7 + 8 + 13）

### Slide 1：实验目标
- **问题**：如何定时跑任务并暴露服务？

### Slide 2：创建 Job 打印斐波那契
```yaml
apiVersion: batch/v1
kind: Job
```

### Slide 3：创建 CronJob 每分钟打印时间
```yaml
schedule: "*/1 * * * *"
```

### Slide 4：部署 Web 应用
```yaml
apiVersion: apps/v1
kind: Deployment
```

### Slide 5：创建 Ingress 暴露服务
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
```

---

## 📅 Week 15：讲授 - 如何驾驭有状态的世界（PDF Chapter 12）

### Slide 1：问题引入
- **问题**：如何部署 MySQL 集群？
- **图**：图 12-1 “StatefulSet architecture”（PDF Chapter 12, Page 120）

### Slide 2：StatefulSet 三大保证
- **内容**：稳定身份、独立存储、有序部署
- **代码**（PDF Chapter 12, Page 118）：
```yaml
apiVersion: apps/v1
kind: StatefulSet
spec:
  serviceName: mysql
  replicas: 3
```

### Slide 3：Headless Service
- **代码**（PDF Chapter 12, Page 120）：
```yaml
clusterIP: None
```

### Slide 4：volumeClaimTemplates
- **代码**（PDF Chapter 12, Page 119）：
```yaml
volumeClaimTemplates:
- metadata:
    name: data
  spec:
    accessModes: ["ReadWriteOnce"]
    resources:
      requests:
        storage: 10Gi
```

### Slide 5：Pod 命名规则
- **内容**：mysql-0, mysql-1, mysql-2
- **图**：图 12-1 StatefulSet Pod 命名（PDF Chapter 12, Page 120）

---

## 📅 Week 16：实验 - 综合项目实战（整合所有模式）

### Slide 1：项目结构
- **内容**：前端 + 后端 + 数据库
- **图**：图 12-1 三层架构图（PDF Chapter 12, Page 120）

### Slide 2：部署 MySQL StatefulSet
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  replicas: 1
  serviceName: mysql
```

### Slide 3：部署后端 Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: app
        image: backend:1.0
```

### Slide 4：创建 Ingress 暴露前端
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: frontend
spec:
  rules:
  - host: blog.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend
            port:
              number: 80
```

### Slide 5：部署 CronJob 每日备份数据库
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup-mysql
spec:
  schedule: "0 2 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: mysql:8.0
            command:
            - mysqldump
            - -h mysql
            - -u root
            - -p$MYSQL_ROOT_PASSWORD
            - blog > /backup/blog.sql
```

---

## ✅ 图例清单（供 PPT 引用）

| 图名 | PDF 章节 | 页码 | 图编号 |
|------|-----------|------|--------|
| 云原生路径图 | Chapter 1 | 2 | 图 1-1 |
| Pod 结构图 | Chapter 1 | 6 | 图 1-2 |
| 标签分组图 | Chapter 1 | 8 | 图 1-3 |
| K8s 开发者资源图 | Chapter 1 | 11 | 图 1-4 |
| QoS 类型表 | Chapter 2 | 21 | 表 2-1 |
| RollingUpdate 图 | Chapter 3 | 32 | 图 3-1 |
| Blue-Green 图 | Chapter 3 | 34 | 图 3-3 |
| Canary 图 | Chapter 3 | 35 | 图 3-4 |
| Job 流程图 | Chapter 7 | 81 | 图 7-1 |
| StatefulSet 架构图 | Chapter 12 | 120 | 图 12-1 |
| Pod 调度图 | Chapter 6 | 73 | 图 6-3 |
| 无状态架构图 | Chapter 11 | 113 | 图 11-1 |

