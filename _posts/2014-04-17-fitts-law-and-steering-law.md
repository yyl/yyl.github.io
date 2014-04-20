---
layout: post
title:  "Reading: Beyond Fitts' law: Models for Trajectory-based HCI tasks"
date:   2014-04-17 23:10:10
categories: HCI
---

**The problem** Fitts' law is a good mathematical model for HCI study, but only fit for one specific movement: pointing. What about other tasks performed in the interaction of human and computers?

**The solution** extend Fitts' law to cover the performance evaluation of trajectory-based tasks. The authors give mathematical equations derived from Fitts' law to characterize the task under four different conditions. The process is in form of four different experiments.

## What is Fitts' law

Fitts' law is a mathematical model to describe human movement. In particular, for a human pointing task, it is to represent the time needed to perform the task by two variables

    MT = a + b * log2((A/W) + c)


- `MT` is movement time
- `A` is the _distance_ from the starting point to the center of the target
- `W` is the width of the target _along the axiom of motion_
- `a` and `b` are empiracally determined constant and `c` is 0, 0.5 or 1

In fact, people always treat `log2((A/W) + c)` part as a whole, and call it **index of difficulty (ID)**; ID represents the difficulty of the task given the design (`A` and `W`). Fitts' law forms the model of speed/accuracy in pointing task.

### What is a human pointing task in the Fitts' law model

The pointing task could be explained by the follwing figure I found from [this blog](http://www.particletree.com/features/visualizing-fittss-law/); it also has a great explanation about Fitts' law and its caveats.

![Pointing task]({{ site.url }}/images/pointing.gif)

All in all, Fitts' law tells/matches one intuition:

> the wider the target/the closer to the target, the faster the task could be performed

Note though, their relationship is `log` function, therefore it is not always bigger the better. Increasing the target width from 1cm to 10cm might significantly reduce the `MT`, increasing it from 1m to 10m might not get the same propotion of reduction in `MT`.

Aside from this, the famous blog CodingHorror has two blogs ([1](http://blog.codinghorror.com/the-opposite-of-fitts-law/),[2](http://blog.codinghorror.com/fitts-law-and-infinite-width/)) telling a few very interesting observations from Fitts' law.

## Trajectory-based task: steering

The main task this paper studies is called "steering": move the object (e.g. mouse, finger) through bounded tunnel. It is illustrated in figure below.

![Steering task]({{ site.url }}/images/steering.png)

The paper uses four experiments to derive a common model for such type of task. The four experiments are:

1. goal passing: like a steering but only start and end of the tunnel is bounded, which is called 2 "goal"s
2. constraint-added goal passing: when we add infinite "goal"s to the tunnel
3. narrowing tunnel: 2 ends have different width
4. spiral tunnel: curve paths

All experiments are fully-crossed, within-subjects factorial design. Every participant went through all experiments in random order.

With the modeling of results from four experiments, the authors derived a _global law_ for a generic steering task,

    MT = a + b * (A/W)

Basically, the authors concluded that, instead of a `log` relationship like the pointing task has, for steering tasks, the difficulty/time and the design (being `A` and `W`) is linear. They also derived a _local law_, which is similar to the global law. The main take-home message of local law is that instantaneous speed of steering at any point is proportional to the variability permitted.

Linear relationship seems odd, because in real design `MT` cannot possibly increase unlimited as long we widen the path. Therefore, the authors stated that there existed an _upper bound_ for the path width in steering task. Also, in discussion they talked about the possibility of taking path curvature and path beginning into account.
