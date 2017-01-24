# kubeTransition-schedule
“计划经济”下的kubernetes的调度策略

## 背景

在传统的应用部署上，可能使用的是指定机器IP部署的策略，而kubernetes的docker编排采用的是“市场经济”策略，根据集群中机器的状况进行筛选、打分，选取最优的机器进行部署docker。从传统的应用部署方式转换到kubernetes的docker编排方式的过程中，可能需要一个过渡的阶段，即对应用进行计划分配IP，所以这个项目的名称为**kubeTransition-schedule**

## 功能

* 在kubernetes中指定某些机器用于部署该pod
* pod之间的互斥问题（亲和性）
* pod独占机器部署（一台机器上只能部署一个pod）

## kubernetes中的schedule模块源码分析

[kubernetes的github地址](https://github.com/kubernetes/kubernetes)

源码版本：1.3.10

kube-schedule的源码在/kubernetes/plugin下，以下是plugin目录下kube-schedule的代码结构（去掉了其他不相干的代码）

	plugin
	├── cmd
	│   └── kube-scheduler
	│       ├── app
	│       │   ├── options
	│       │   │   └── options.go
	│       │   └── server.go //SchedulerServer的数据结构定义
	│       └── scheduler.go  //kube-scheduler的入口程序
	└── pkg
    	└── scheduler
   	 	    ├── algorithm //预选和优选策略算法
    	    │   ├── doc.go
    	    │   ├── listers.go
    	    │   ├── predicates
    	    │   │   ├── error.go
   	 	    │   │   ├── predicates.go
    	    │   │   └── predicates_test.go
    	    │   ├── priorities
    	    │   │   ├── interpod_affinity.go
    	    │   │   ├── interpod_affinity_test.go
	   	    │   │   ├── metadata.go
    	    │   │   ├── node_affinity.go
    	    │   │   ├── node_affinity_test.go
    	    │   │   ├── priorities.go
    	    │   │   ├── priorities_test.go
    	    │   │   ├── selector_spreading.go
    	    │   │   ├── selector_spreading_test.go
    	    │   │   ├── taint_toleration.go
    	    │   │   ├── taint_toleration_test.go
    	    │   │   └── util
    	    │   │       ├── non_zero.go
    	    │   │       ├── topologies.go
    	    │   │       └── util.go
    	    │   ├── scheduler_interface.go
    	    │   ├── scheduler_interface_test.go
    	    │   └── types.go
    	    ├── algorithmprovider
    	    │   ├── defaults
    	    │   │   ├── compatibility_test.go
    	    │   │   └── defaults.go
    	    │   ├── plugins.go
    	    │   └── plugins_test.go
    	    ├── api
    	    │   ├── latest
    	    │   │   └── latest.go
    	    │   ├── register.go
    	    │   ├── types.go
    	    │   ├── v1
    	    │   │   ├── register.go
    	    │   │   └── types.go
    	    │   └── validation
    	    │       ├── validation.go
    	    │       └── validation_test.go
    	    ├── equivalence_cache.go
    	    ├── extender.go
    	    ├── extender_test.go
    	    ├── factory
    	    │   ├── factory.go
    	    │   ├── factory_test.go
    	    │   ├── plugins.go
    	    │   └── plugins_test.go
    	    ├── generic_scheduler.go
    	    ├── generic_scheduler_test.go
    	    ├── metrics
    	    │   └── metrics.go
    	    ├── scheduler.go
    	    ├── scheduler_test.go
    	    ├── schedulercache
    	    │   ├── cache.go
    	    │   ├── cache_test.go
    	    │   ├── interface.go
    	    │   ├── node_info.go
    	    │   └── util.go
    	    └── testing
    	        ├── fake_cache.go
    	        └── pods_to_cache.go
  
