# Probabalistic Aggregation Strategies in Swarm Robotic Systems

<strong>Summarised By:</strong> Arjun Puthli 

Refer to paper [here](https://ieeexplore.ieee.org/document/1501639)

---

## About the paper

This paper focuses on modelling individual, local robot behaviours such that they are transformed into a globally optimum behaviour. In this case, the focus being on controlling the aggregation and clustering behaviour of a swarm of robots making use of a simple probabalistic finite state machine.

## Algorithm Summary

The controller consists of two layers: A higher level consisting of the robot approach, repel and waiting behaviours and a lower level obstacle avoidance behaviour. The lower level obstacle avoidance behaviour depends on infra-red sensor information, and overall doesn't contribute to the swarm aggregation. Therefore the focus is on the finite state machine of the higher level, which works as follows:

As mentioned earlier the higher level consists of _approach_, _repel_ and _wait_ behaviours. They can be described as:

* <strong>Approach:</strong> The robot uses sound sensors to estimate the relative direction of the loudest noise source and drives in that direction.

* <strong>Wait:</strong> The robot stays in place

* <strong>Repel:</strong> Acts opposite to _approach_, and drives the robot in the opposite direction of the loudest sound source.

Every robot starts out in the approach state. It continues to be in this state until it is able to detect at least one other robot (with infrared sensors), and then switches to the wait state. This condition is termed as the "_robot close_" condition in the paper. 

Consider fixed values $P_{return}$ and $P_{leave}$ $\epsilon$ $[0,1]$

During the wait state the robot samples a value $P$ uniformly as $U \sim [0,1] $. If $P < P_{leave}$ then the robot moves to the repel state. Else it continues in the wait state.

In the repel state, if $P < P_{return}$, then robot moves to the approach state, else it continues in the repel state.

## Performance Metrics

1. <strong>Expect Cluster Size (ECS):</strong> Makes use of a threshold value $T_{RobotClose}$, such that if the distance between any two robots $d$ is less than $T_{RobotClose}$, they are considered neighbours. The cluster size for a robot $R_i$ is defined as $Size({R_i})$, which is the number of robots in the cluster that robot belongs to. Hence ECS can be defined as follows: $$ECS = \frac{1}{n}\sum_{i=1}^{n}{Size^2({R_i})}$$ 
This metric ignores spatial distribution of the clusters, but is a measure of the size of the clusters.

2. <strong>Total Distance: </strong> This provides more information regarding spatial configuration of the swarm and the clusters. It uses negative distance to emphasize better clustering for lower distance values. The metric is defined as follows: $$TD = -\sum_{i=1}^n \sum_{j=i+1}^n dist(R_i, R_j)$$

## Effects of changing the parameters

1. $P_{leave} \neq 0, P_{return} \neq 0$: Increasing $P_{leave}$ reduces the size of the clusters and increases the number of robots searching for a cluster. On the other hand increasing $P_{return}$ increases the cluster size. 

2. $P_{leave} = 0$: In this strategy, when a robot nears another robot, it stops and waits there forever since the probability of switching to the repel state is 0. This leads to frozen aggregation, and expected cluster size is independent of $P_{return}$.

3. $P_{return} = 0$: All robots move to the repel state and no clustering occurs.

4. $P_{return} = P_{leave} = 1$: Similar to strategy 1, but the robots neve enter the wait state. 

