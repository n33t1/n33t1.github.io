---
layout: post
title: "Reinforcement Learning"
description: "Notes for Reinforcement Learning chapters from CS188x"
date: 2017-07-04
tags: [research, deep learning]
comments: true
share: true
---
## Reinforcement Learning
Returns state and rewards
Agents utility is defined by the reward function
Goal is to maximize expected rewards
All learning is based on observed samples


Offline solution, online learning
Still assumes a Markov decision process(MDP), but we dont know model T or reward R

#### Terminologies: 
* T: model
* R: rewards
* alpha: learning rate
* Q-value: value of an action

### Model-Based and Model-Free Learning
When P(A) is unknown and we are trying to reconstruct it, we can use either “Model Based” or “Model Free” approaches. The former will learn the probability distributions and then reduce it to the case of solving a known MDP. The latter will calculate the frequency and update it correspondingly as we go. 

### Model-Based Learning

Learn an approximate model based on experiences (so still observational study??)
Solve for values as if the learned model were correct

Step 1: Learn empirical MDP model
Let the program run for a while then count outcomes s’ for (state, action). Then, we statistically normalize to give an estimate model T(s,a,s’). With this transition function T, we discover the corresponding rewards R(s,a,s’).

Step 2: Solve the learned MDP
For example, use value iteration, as before 

### Model-Free Learning

#### Passive Reinforcement Learning
Simplified task: policy evolution

#### Directed Evaluation 
Goal is to computer values, i.e. sum(discounted rewards) for each state under pi
We can us DP (memoization) to go between states

* Pros: easy to understand and doesnt require knowledge of T, R
* Cons: each s must be learned separately and the result doesnt reflect state connections

In order to fix it, aka to add connections between states, we use Bellman’s formula, which unfortunately requires T and R. The key question becomes how do we take a weighted average without knowing the weights.

#### Temporal Difference Learning
Big Idea: Model-free way to do policy evaluation, mimicking Bellman updates with running sample averages. Learn from every experience, i.e. update V(s) each time w experience a transition (s,a,s’,r)
Exponential moving average. It makes recent samples more important and forgets about the past. 

* Experience world through episodes
* Update estimates each transition
* Over time, updates will mimic Bellman updates

#### Active Reinforcement Learning
You still dont know T and R, but you take the actions now.    
Goal: learn the optimal policy / values

#### Q-learning: sample-based Q-value iteration

Learn Q(s, a) values as you go: 
For each state, you will receive a new sample (s, a, s’, r). Then you update your new sample estimate by combining the new reward and the old estimate. Then interpolate the new estimate into the running average. 

Properties: Q-learning converges to optimal policy — even if youre acting suboptimally! (Off-policy learning)

### How to explore? 
#### ramdom actions (epsilon=greedy)
* Every step, flip a coin
* With small probability epsilon, act randomly
* With large probability 1 - epsilon, act on current policy

### Problems with random actions?
* You do not eventually explore the space, but keep thrashing around once learning is done
* You can either lower epsilon over time or add exploration functions

#### Exploration Functions
Explore areas whose badness is not yet established, and eventually stop exploring

Exploration Function:
f(u, n) = u + k /n
where u = utility estimate, n = visit count

### Regret
* Making mistakes is inevitable
* It is a measure of your total mistake cost: diff(expected rewards vs optimal rewards)
* Minimixing regret goes beyond to be optimal

### Generalizing Across States
IRL, its impossible to learn all the states (too many states or MLE)

Instead, we want to generalize the new experience to new, similar situations

### Feature-Based Representations
* Solution: describe a state using a vector of features
* Linear Value Functions

### Approximate Q-Learning
* Q-learning with linear Q-functions
transition = (s,a,r,s')
diff = [r+ average Q-value] - current Q-value

### Q-Learning and Least Squares
* Error function
* Linear function can prevent overfit

### Policy Search
Start with an ok solution (e.g. Q-learning) then fine-tune by hill climbing on feature weights. Here is where machine learning starts (?)

