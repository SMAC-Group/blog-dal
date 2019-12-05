---
author: "Samuel Orso, Lionel Voirol"
title: "High Performance Computing - Parallel computing on Baobab"
date: 2019-12-05
description: "Parallel computing in Baobab HPC cluster for researchers"
tags: ["computational statistics"]
categories: ["tutorial"]
featured_image: "parallel.jpg"
---

# Motivation & Introduction
Researchers at the University of Geneva may request access and use the high-performance computing (HPC) server of the University of Geneva, Baobab. This cluster is particularly well suited for massive parallel computations. There exist already different ressources to help the user that are listed at the end of this tutorial.
However, the documentation already provided can be challenging to grasp for a first use or a new user. The aim of this tutorial is therefore to provide an clear and concise introduction to the use of the HPC Cluster Baobab.

# Parallelisation and optimal scheduling
The `bash` script discussed in the previous tutorial run the `R` script written below on one CPU in one node. There is therefore no parallelisation of the code. However, when each iteration on a given programm are independant, one can paralellise the code such that the run time is reduced. We will in this tutorial discuss both the notion of command parallelisation and parallelisation via slurm, how one should modify both scripts based on the type of parallelisation and how the code will be executed on Baobab.

## Command parallelisation
To be done

## Parallelisation via Slurm

To be done

# Useful ressources

 * [What is Baobab](https://plone.unige.ch/distic/pub/hpc/baobab_en)
  * [Baobab Documentation](https://baobab.unige.ch/enduser/enduser.html)
  * [Slurm Documentation](https://slurm.schedmd.com/)
  * [Slurm Scheduling for Optimal Responsiveness and Utilization](https://slurm.schedmd.com/SUG14/sched_tutorial.pdf)
