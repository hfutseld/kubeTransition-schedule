# kubeTransition-schedule
“计划经济”下的kubernetes的调度策略

## 背景

在传统的应用部署上，可能使用的是指定机器IP部署的策略，而kubernetes的docker编排采用的是“市场经济”策略，根据集群中机器的状况进行筛选、打分，选取最优的机器进行部署docker。从传统的应用部署方式转换到kubernetes的docker编排方式的过程中，可能需要一个过渡的阶段，即对应用进行计划分配IP，所以这个项目的名称为**kubeTransition-schedule**

## 功能

* 在kubernetes中指定某些机器用于部署该pod
* pod之间的互斥问题（亲和性）

## kubernetes中的schedule模块源码分析

[kubernetes的github地址](https://github.com/kubernetes/kubernetes)

源码版本：1.4以后 