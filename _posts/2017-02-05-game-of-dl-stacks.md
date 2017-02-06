---
layout: post
title: "Game of DL Stacks"
description: "A research on machine intelligence tools for research or engineering among Caffe, MXNet, Torch/PyTorch and TensorFlow"
date: 2017-02-05
tags: [deep learning, research]
comments: true
share: true
---

Hook blablabla. This article will compare four Machine Learning (ML) libraries that was popular on the Internet. The comparison matrixes is inherited from [DLIF(Deep Learning Implementations and Frameworks) tutorial](https://sites.google.com/site/dliftutorial/), [Benchmarking State-of-the-Art Deep Learning Software Tools](https://arxiv.org/pdf/1608.07249v6.pdf), and [Caffe: Convolutional Architecture for Fast Feature Embedding](http://ucb-icsi-vision-group.github.io/caffe-paper/caffe.pdf). The goal is to understand the unique pros and cons of the tech stacks and what they are most useful for, and then choose one for performing experiment for ML hardware research.

![alt text](https://raw.githubusercontent.com/n33t1/n33t1.github.io/master/images/Screen%20Shot%202017-02-06%20at%202.39.17%20AM.png)
Framework Comparison among various machine learning frameworks from DLIF presentation[^1]

[^1]: <https://www.dropbox.com/s/8l5faz0x2588x4o/AAAI2017-3-0203.pdf>

![alt text](http://en.community.dell.com/resized-image/__size/1650x0/__key/communityserver-blogs-components-weblogfiles/00-00-00-45-39/image007.png)      
Speedup Comarison for various machine learning frameworks made by the Dell Lab [^2]

[^2]: <http://en.community.dell.com/techcenter/high-performance-computing/b/general_hpc/archive/2016/11/11/deep-learning-performance-with-p100-gpus>      

### Caffe
* Features:
	* Write NNs in declarative configuration files (Framework builds layers of NNs as written in the files)
	* Backpropthrough graphs (Framework only builds graphs of forward prop, and do backpropby backtracking the graphs)
	* Parameters as part of operator nodes (shows layer instead of individual nodes)
	* Update parameters by own routines outside of the graphs (Update formulae are implemented directly using the backend array libraries)
	* Static computational graphs
* Deveoped by BVLC (U.C.Berkeley)
* Language Stacks(Core/User Languages):
	* C++
    * C++, Python, MATLAB
* Pros:
	* NNs is easy to parse and reuse for other frameworks
	* Backprop is easy to implement
	* Easy to update parameters 
	* Easy to optimize the computations

* Cons:
    * NNs is hard to write when it gets complicated
    * Backprop may lose features that are available for graphs
    * Updated formulae are not integreted to computational graphs
    * Hard to build different graphs for different iterations
    * [Yangqing Jia](http://daggerfs.com/) joined the TF team now 
* Noticeable Users: 
    * [Berkeley BAIR Lab](http://bair.berkeley.edu/software.html)
    * [MIT EEMS group](http://www.rle.mit.edu/eems/research/), and check out their [Eyeriss Caffe project](http://eyeriss.mit.edu/)

### TensorFlow
* Features:
	* Writes NNs by procedural scripting (Framework provides APIs of scripting languages to build NNs)
	* Backpropas extended graphs (Framework builds graphs for backpropas well as those for forward prop) 
	* Parameters as separate nodes in the graphs (shows individual nodes instead of layer)
	* Represents update formulae as a part of the graphs (Update formula are built as a part of computational graphs)
	* Statics computational graphs
	* Transform the graphs to optimize the computations
	* Supports computational graph optimizations
	* Supports distributed computation to further scale the learning by using gRPC
	* Processing speed is comparatively low [^3]
	
[^3]: <http://en.community.dell.com/techcenter/high-performance-computing/b/general_hpc/archive/2016/11/11/deep-learning-performance-with-p100-gpus>
* Deveoped by Google
* Language Stacks(Core/User Languages):
	* C++, Python
    * C++, Python
* Pros:
	* NNs is easy to implement
	* Backprop can use any features that are available for graphs
	* Represent nodes inituitively
	* Implementation gets complicated as framework must support mutable operations within the computational graphs
	* Easy to optimize the computations
* Cons:
	* NNs is hard to port to other frameworks 
	* Backprop might be hard to implement
	* Parameters has low flexibility and reusability
	* We can apply e.g. optimizations to the update formulae
    * Hard to build different graphs for different iterations
* Noticeable Users: 
    * OpenAI [https://openai.com/blog/]
    * DeepMind [https://research.googleblog.com/2016/04/deepmind-moves-to-tensorflow.html]

### Torch/PyTorch
* Features:
	* Writes NNs by procedural scripting
	* Backpropthrough graphs
	* Parameters as part of operator nodes
	* Updates parameters by own routines outside of the graphs
	* Torch is Static computational graphs, but PyTorch is Dynamic computational graphs
	* Provide ways to write one code that runs both on CPU and GPU
	* PyTorch supports distributed computation to further scale the learning
* Deveoped by Facebook, Twitter, ...
* Language Stacks(Core/User Languages):
	* C/Lua for Torch, C++, Python for PyTorch
    * Lua for Torch, Python for PyTorch
* Pros:
	* NNs is easy to implement
	* Backprop is easy to implement
	* Easy to update parameters 
	* For PyTorch, the user can build different graphs for different iterations using language syntaxes
* Cons:
	* NNs is hard to port to other frameworks
	* Backprop may lose features that are available for graphs
	* Updated formulae are not integreted to computational graphs
	* For PyTorch, iterations are hard to optimize
* Noticeable Users: 
    * OpenAI [https://openai.com/blog/]
    * DeepMind [https://research.googleblog.com/2016/04/deepmind-moves-to-tensorflow.html]

### MXNet
* Features:
	* Writes NNs by procedural scripting
	* Backpropthrough graphs
	* Parameters as part of operator nodes
	* Updates parameters by own routines outside of the graphs
	* Static computational graphs
	* Provides ways to write one code that runs both on CPU and GPU
	* Supports distributed computation to further scale the learning by using a simple distributed key-value store
	* Has the fastest processing speed[^3]
* Deveoped by DMLC
* Language Stacks(Core/User Languages):
	* C++
    * C++, Python, R, Go, ...
* Pros:
	* NNs is easy to implement
	* Backprop is easy to implement
	* Easy to update parameters 
	* Easy to optimize the computations
* Cons:
	* NNs is hard to port to other frameworks
	* Backprop may lose features that are available for graphs
	* Updated formulae are not integreted to computational graphs
	* Hard to build different graphs for different iterations
* Noticeable Users: 
    * OpenAI [https://openai.com/blog/]
    * DeepMind [https://research.googleblog.com/2016/04/deepmind-moves-to-tensorflow.html]
