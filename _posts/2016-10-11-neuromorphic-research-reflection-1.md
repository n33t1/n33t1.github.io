---
layout: post
title: "Neuromorphic Research Reflection - Week 1"
description: "A basic view"
date: 2017-01-20
tags: [deep learning, research]
comments: true
share: true
---

# eNVM

eNVM (emerging nonvolatile memory) is a novel type of computer memory that can retrieve stored information even the power is removed. It is one possible method to achieve hardware-level neuromorphic computing, which is a new computer architecture. Memristor is the most popular eNVM technology using in neuromorphic computing.

### Memristor

The imaginary forth element of fundamental circuit elements. It uniquely defines the relationship between magnetic flux and electrical charge. Memristance is the resistance state of a memristor, which can be tuned by applying an electrical excitation. (http://www.nature.com/nature/journal/v453/n7191/fig_tab/453042a_F1.html)

The invention of memristor crossbar is what makes programming connection in neural networks possible. Every input neuron can connect to all output neurons via a crossbar, and a synaptic weight is represented by the resistance of the memristor at the corresponding cross-point. Training algorithms of neural network models can possibly be developed to program the resistance of the memristors in the crossbar. In summary, the output neuron y equals to the product of the weight of synapse net W and input neurons u.

### Software Design
In eNVM-enabled neuromorphic computing system (NCS), in a software design technique perspective, the accuracy of the neural networks by:
 1) Vortex, which is a variation-aware robust training scheme that can tolerate the memristor device variations using 1) Variation-aware training (VAT): calculate the variation first, adjust before the actual execution; 2) Adaptive mapping (AMP): adjusting while executing. 

 2) Digital-assisted training initialization step, which is a method to improve the convergence speed of MBC by setting the initial resistance of the memristors close to the target value. The robustness of the training process is gradually improved. 

3) Optimizing cost function using regularizations (regularization is referred to as a process of introducing additional information to prevent overfitting). 

* Definition: In general, the regulatization term can be expressed by a complexity penalty added to the cost function of the learning process. The function cost J' equals to the sum of the original function cost and the product of the importance of the regularization term and the added complexity. 

* Methods: L1-norm regularization and L2-norm regularization.

* Structured Sparsity Learning (SSL) is a method to regularize the structures of Deep Neural Networks (DNN).

* Possible research direction: For MNIST applications, the tolerance to quantization loss still demands improvements, esp. for binary representations.

### Hardware Design
1) A digitalized interface in spikes with good noise immunity and energy efficiency for signal transferring.
2) Highly-efficient parallel analog operations of synaptic cost function through resistive crossbar arrays.
3) A novel integrate-and-fire circuit converting the analog computation data to output spikes at a rate of up to 568.2M spikes/sec and energy of 0.48pJ-per-spike. 

Possible research direction: How to reduce power consumptions and improve the power efficiency of NCS - > 1) implement neuromorphic-specific hardware; 2) optimizing hardware-aware algorithms


___
Work Cited: 
Song, Chang, Beiye Liu, Chenchen Liu, Hai Li, and Yiran Chen. "Design techniques of eNVM-enabled neuromorphic computing systems." 2016 IEEE 34th International Conference on Computer Design (ICCD) (2016): n. pag. Web.
