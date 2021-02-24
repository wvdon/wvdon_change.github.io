---
title: 'Microscaler: Automatic Scaling for Microservices with an Online Learning Approach'
date: 2021-1-19 14:24:25
tags: 
	- 
categories: [paper]
description: 每日论文翻译
---

- Microscaler ：使用在线学习方法对微服务进行自动度量
- 使用在线学习和启发式方法，去实现最优的服务规模。以满足负载均衡
- 余广坝,徐鹏飞,郑子彬 etc.
- 来源  https://ieeexplore.ieee.org/ .
- 使用doi 下载链接  https://sci-hub.se/10.1109/ICWS.2019.00023

Abstract—*Recently, the microservice becomes a popular architecture to construct cloud native systems due to its agility. In*

*cloud native systems, autoscaling is a core enabling technique to adapt to workload changes by scaling out/in. However, it becomes a challenging problem in a microservice system, since such a system usually comprises a large number of different micro services with complex interactions. When bursty and unpredictable workloads arrive, it is difficult to pinpoint the scaling-needed services which need to scale and evaluate how much resource they need. In this paper, we present a novel system named Microscaler to automatically identify the scaling-needed services and scale them to meet the service level agreement (SLA) with an optimal cost for micro-service systems. Microscaler collects the quality of service metrics (QoS) with the help of the service mesh enabled infrastructure. Then, it determines the under-provisioning or over-provisioning services with a novel criterion named service power. By combining an online learning approach and a step by step heuristic approach, Microscaler could achieve the optimal service scale satisfying the SLA requirements. The experimental evaluations in a micro-service benchmark show that Microscaler converges to the optimal service scale faster than several state of the art methods.*

翻译：

摘要：最近，由于微服务的敏捷性，他变成了构成本地云系统的一种流行结构。在本地云系统，自动度量是一种通过度量进出来调整工作负载机会的核心技术。然而，自从系统通常使用复杂的交互在不同的大量微服务中以来，微服务系统中他变成了一个挑战。当突发和不可预测的工作负载到达，那是很难去指出微服务需要多少度量并且评估他们需要多少资源。在这篇文章中，我们呈现了一个新系统，并将其命名为Mircroscaler，去自动识别这个服务度量需要，并且对于微服务系统使用一个优化消费去衡量他们，以满足服务水平协议。Microscaler 在启用服务网格的基础架构的帮助下收集了服务质量指标。然后，他使用名为service power的新标准去确定支配不足或过度支配服务。通过联合在线学习的方法和步步为营的启发式方法，Microscaler 可以实现这个最优服务规模，满足SLA的要求。在微服务基准中，实验评估表明:Microscaler收敛到最近服务规模的速度比先进最先进的方法都要快。



- agility 敏捷性
- comprises 包括
- bursty 突发
- optimal 优化
- the service level agreement  服务水平协议
- the quality of service metrics (QoS)   服务质量指标
- **the service mesh enabled infrastructure**  **启用服务网格的基础架构**
- heuristic 启发式
- under-provisioning 预配不足
- criterion  标准
- benchmark  基准
- state of the art 最先进的
- converges 收敛