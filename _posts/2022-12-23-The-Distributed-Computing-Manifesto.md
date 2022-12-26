---
layout: post
title:  Thoughts on The Distributed Computing Manifesto
date:   2022-12-23 20:16:00 +0800
---

## Thoughts on The Distributed Computing Manifesto
* The Distributed Computing Manifesto
https://www.allthingsdistributed.com/2022/11/amazon-1998-distributed-computing-manifesto.html

### Old architecture
> Our current two-tier, client-server architecture is one that is essentially data bound. The applications that run the business access the database directly and have knowledge of the data model embedded in them. This means that there is a very tight coupling between the applications and the data model, and data model changes have to be accompanied by application changes even if functionality remains the same. This approach does not scale well and makes distributing and segregating processing based on where data is located difficult since the applications are sensitive to the interdependent relationships between data elements.
  * access DB directly, tight coupling
  
### Two key concepts
  * Service-based Model
  * Workflow-based Model

### Service-based model
> We propose moving towards a three-tier architecture where presentation (client), business logic and data are separated. This has also been called a service-based architecture. The applications (clients) would no longer be able to access the database directly, but only through a well-defined interface that encapsulates the business logic required to perform the function.

> Services, in combination with workflow, will have to provide both synchronous and asynchronous methods. 

#### Asynchronous request - polling and callback
对于异步asynchronous request, 作者提到两种requestor获取result的方式:
* polling
* callback

这篇文章[^1]提到了第三种方式: requestor提供一个接收result的queue

### Workflow-based Model
> Amazon's business is well suited to a workflow-based processing model. We already have an "order pipeline" that is acted upon by various business processes from the time a customer order is placed to the time it is shipped out the door. Much of our processing is already workflow-oriented, albeit the workflow "elements" are static, residing principally in a single database.
* Amazon电商业务适合workflow-based processinng model  
* 已有"order pipeline"来管理customer order 的创建和ship out

> An example of our current workflow model is the progression of customer_orders through the system. The condition attribute on each customer_order dictates the next activity in the workflow. 
* 看起来workflow elements是静态的存在DB中,类似条件变量存储当前的状态. 代码加载DB中order数据, 决定workflow下一跳是什么, 之后再写回DB. 下面的例子也提到类似实现. 

> However, the current database workflow model will not scale well because processing is being performed against a central instance. As the amount of work increases (a larger number of orders per unit time), the amount of processing against the central instance will increase to a point where it is no longer sustainable.
  * 在当时(1998年)应该是没有分布式数据库, 所以数据存储无法做有效的scale up, 当时order量很大时, 数据的读写会受限. 同时如果数据库出现问题, 那么所有的order处理都会受影响.
  * `This is not to say a database-backed workflow is superior, only that the database technology is not the limitation nowadays.`[^1]提到了DynamoDB, 分布式数据存储可以做scale up, 从另一个角度也能解决这个问题.
  
> A solution to this is to distribute the workflow processing so that it can be offloaded from the central instance. Implementing this requires that workflow elements like customer_orders would move between business processing ("nodes") that could be located on separate machines.
  * 作者提到的一个solution: order move between business processing node. 而这些processing node 是分布式的, 在不同机器上. 这里有点AWS StepFunction的雏形概念, data在workflow node间流动, 而每个处理node的计算资源是分布式的.

> Instead of processes coming to the data, the data would travel to the process. This means that each workflow element would require all of the information required for the next node in the workflow to act upon it. This concept is the same as one used in message-oriented middleware where units of work are represented as messages shunted from one node (business process) to another.
  * **Instead of processes coming to the data, the data would travel to the process.**
  * 这里workflow的处理和基于message在各个business processing node之间传递是一致的

#### How workflow is directed? 谁来指挥workflow运行?
> An issue with workflow is how it is directed. Does each processing node have the autonomy to redirect the workflow element to the next node based on embedded business rules (autonomous) or should there be some sort of workflow coordinator that handles the transfer of work between nodes (directed)? 
  * autonomous: 每个node中embeded business rules, 来决定下一跳去哪个node
  * directed: 有一个统一的coordinator来决定下一个node是什么
    * 这种方式每个node都像是一个service, 对输入返回result, coordinator/requestor来决定如何处理result, 这样使得每个node不依赖其他node, node的操作更加general, 使得整个workflow flexibility更好. 一些nodes完全可以interchangeable, 甚至可复用的node.
    * 在scalability方面, 如果一个node的处理遇到bottleneck, 就对那个node scale,比如加更多的计算资源
    * 想到AWS StepFunction就是这么做的

## Other readings
1. https://dimosr.github.io/thoughts-on-distributed-computing-manifesto/
1. https://brooker.co.za/blog/2022/11/22/manifesto.html
1. [MapReduce: Simplified Data Processing on Large Clusters](http://static.googleusercontent.com/media/research.google.com/es/us/archive/mapreduce-osdi04.pdf)

## Links
[^1]: https://dimosr.github.io/thoughts-on-distributed-computing-manifesto/
