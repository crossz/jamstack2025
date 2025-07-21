

# Kubernetes设计模式课程大纲 (基于PBL教学法)

## 课程结构
- **总周期**: 16周 (32小时)
- **时间分配**:
  - 单数周: 2小时课堂讲授 (问题驱动)
  - 双数周: 2小时上机实验 (解决问题)
- **图文要求**: 每章节至少5个slide，引用书中images

---

## 调整后章节对应表

| 周次 | 讲授章节 | 实验章节 |
|------|----------|----------|
| 1-3  | Chapter 1: Introduction | - |
| 4-5  | Chapter 2: Predictable Demands | Chapter 2 |
| 6-7  | Chapter 3: Declarative Deployment | Chapter 3 |
| 8-9  | Chapter 4: Health Probe | Chapter 4 |
| 10-11| Chapter 5: Managed Lifecycle | Chapter 5 |
| 12-13| Chapter 6: Automated Placement | Chapter 6 |
| 14-16| Part II+III 模式 (按需调整) | - |

---

## 详细课程安排

### **第1-3周: Chapter 1 - 容器化与云原生基础**

#### **Week 1: 云的演进与容器化**
**讲授内容 (5 slides):**
1. **核心问题**: "如何让我的应用在云端可靠运行？"
   - *图示*: 图1-1 云原生路径 (images/txw1vz.jpg)
2. **容器化解决方案**:
   - 容器 vs 虚拟机
   - Docker基础概念
3. **Kubernetes核心原语**:
   - Pods: 部署单元
   - Services: 服务发现
4. **实验预告**: 下周部署第一个Pod

**实验预告**: 无（纯理论）

---

#### **Week 2: Pod与Service基础**
**讲授内容 (5 slides):**
1. **核心问题**: "如何组织多个容器协同工作？"
   - *图示*: 图1-2 Pod结构 (images/zh5t1g.jpg)
2. **Pod特性**:
   - 共享网络/存储
   - 多容器协作
3. **Service类型**:
   - ClusterIP/NodePort/LoadBalancer
4. **YAML基础**: 第一个Pod定义

**实验预告**: 下周创建Pod与Service

---

#### **Week 3: 命名空间与标签**
**讲授内容 (5 slides):**
1. **核心问题**: "如何隔离不同环境(开发/生产)？"
   - *图示*: 图1-4 开发者概念图 (images/en7h79.jpg)
2. **命名空间(Namespace)**:
   - 资源隔离
   - ResourceQuota示例
3. **标签(Labels)应用**:
   - 调度策略
   - 服务发现选择器
4. **实验预告**: 下周部署带标签的Pod

---

### **第4-5周: Chapter 2 - 可预测的资源需求**

#### **Week 4: 资源声明与QoS**
**讲授内容 (5 slides):**
1. **核心问题**: "如何避免应用因资源不足而崩溃？"
   - *图示*: 表2-1 资源示例 (html表格)
2. **资源请求/限制**:
   - CPU/Memory单位
   - QoS等级 (Guaranteed/Burstable/BestEffort)
3. **示例**: 资源声明YAML
4. **实验预告**: 下周观察资源限制效果

**实验内容 (Week5):**
1. 部署带资源限制的Pod
2. 模拟资源耗尽观察QoS行为
3. *图示*: Node容量计算公式 (Example 6-1)

---

### **第6-7周: Chapter 3 - 声明式部署**

#### **Week 6: 滚动更新与策略**
**讲授内容 (5 slides):**
1. **核心问题**: "如何安全升级应用而不停机？"
   - *图示*: 图3-1 滚动更新 (images/vy0j5x.jpg)
2. **Deployment策略**:
   - RollingUpdate参数 (maxSurge/maxUnavailable)
   - kubectl rollout命令
3. **蓝绿/金丝雀部署**:
   - *图示*: 图3-3/3-4 (images/xnwgn1.jpg, loxj4x.jpg)
4. **实验预告**: 下周部署多版本应用

**实验内容 (Week7):**
1. 创建Deployment并执行滚动更新
2. 使用`kubectl rollout undo`回滚
3. *图示*: 部署策略对比图 (图3-5)

---

### **第8-9周: Chapter 4 - 健康检查**

#### **Week 8: Liveness/Readiness探针**
**讲授内容 (5 slides):**
1. **核心问题**: "如何让K8s知道我的应用是否健康？"
   - *图示*: 图4-1 可观察性选项 (images/wji9qq.jpg)
2. **探针类型**:
   - HTTP/TCP/Exec/gRPC
   - 存活探针 vs 就绪探针
3. **示例YAML**:
   - *图示*: Example 4-1/4-2 (代码片段)
4. **实验预告**: 下周配置探针

**实验内容 (Week9):**
1. 为Nginx部署添加livenessProbe
2. 模拟进程卡死观察自动重启
3. 为应用添加readinessProbe

---

### **第10-11周: Chapter 5 - 生命周期管理**

#### **Week 10: SIGTERM与钩子**
**讲授内容 (5 slides):**
1. **核心问题**: "如何让应用在终止前做清理？"
   - *图示*: 图5-1 生命周期图 (images/gvo3ws.jpg)
2. **核心概念**:
   - SIGTERM/SIGKILL
   - preStop钩子
3. **示例**:
   - *图示*: Example 5-2 (preStop配置)
4. **实验预告**: 下周实现优雅停机

**实验内容 (Week11):**
1. 部署带preStop的Pod
2. 观察终止过程中的等待行为
3. *图示*: 表5-1 生命周期对比

---

### **第12-13周: Chapter 6 - 自动调度**

#### **Week 12: 亲和性与污点**
**讲授内容 (5 slides):**
1. **核心问题**: "如何控制Pod的部署位置？"
   - *图示*: 图6-3 调度依赖图 (images/udd686.jpg)
2. **调度策略**:
   - NodeAffinity/PodAffinity
   - Taints/Tolerations
3. **示例YAML**:
   - *图示*: Example 6-4/6-5 (亲和性配置)
4. **实验预告**: 下周部署到特定节点

**实验内容 (Week13):**
1. 使用nodeSelector部署Pod到特定节点
2. 配置PodAntiAffinity实现高可用
3. *图示*: 图6-2 资源碎片图 (images/do7vkw.jpg)

---

### **第14-16周: 高级模式 (按需调整)**

可选择以下章节:
- Chapter 7: Batch Job (CronJob实验)
- Chapter 10: Singleton Service (StatefulSet)
- Chapter 13: Service Discovery (Ingress)
- Misc: 其他模式 (如Sidecar/Operator)
