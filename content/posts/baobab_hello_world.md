---
author: "Samuel Orso, Lionel Voirol"
title: "Baobab Hello World - High Performance Computing"
date: 2019-12-04
description: "Introduction to the use of Baobab HPC cluster for researchers"
tags: ["computational statistics"]
categories: ["tutorial"]
featured_image: "baobab.jpg"
---

# Motivation & Introduction
Researchers at the University of Geneva may request access and use the high-performance computing (HPC) server of the University of Geneva, Baobab. This cluster is particularly well suited for massive parallel computations. There exist already different ressources to help the user that are listed at the end of this tutorial.
However, the documentation already provided can be challenging to grasp for a first use or a new user. The aim of this tutorial is therefore to provide an clear and concise introduction to the use of the HPC Cluster Baobab.

Before getting interested in how to run tasks on Baobab, it is necessary to define some notions and install some applications. 

 * The administrative procedure required to get access to Baobab is presented [here](https://baobab.unige.ch/enduser/src/enduser/access.html#access-baobab).
 * Depending on your OS, you may access Baobab and transfer files between your computer and your LINUX session on Baobab using different softwares. Read and install the required softwares [here](https://baobab.unige.ch/enduser/src/enduser/access.html#access-baobab).
 * As indicated above, to work with Baobab, you will be coding instructions on a LINUX command prompt, typing BASH commands. We will list in this tutorial the most frequently used BASH commands when working with Baobab.
 * Regarding the architecture of Baobab, one must understand that Baobab is composed of different partitions, each partition composed of different nodes, for which, each node is composed of a number of CPU/GPU. One can find further details about partitions and their various limits [here](https://baobab.unige.ch/enduser/src/enduser/enduser.html#).

# Useful BASH commands

Command syntax        Description  
----------------      ------------------------------
ls -l                 list current directory
pwd                   print working directory
cd ~                  navigate to home directory
cd ..                 navigate up one directory
cp oldfile newfile    make a copy of a file 
mv oldfile newfile    rename a file
rm file               delete a file

# Your first bash script to execute an R script

# Command parallelisation

# Parallelisation via Slurm

# Installing R packages

# Useful ressources

 * [What is Baobab](https://plone.unige.ch/distic/pub/hpc/baobab_en)
  * [Baobab Documentation](https://baobab.unige.ch/enduser/enduser.html)
  * [Slurm Documentation](https://slurm.schedmd.com/)
  * [SchedMD LLChttp://www.schedmd.comTuning Slurm Scheduling for Optimal Responsiveness and Utilization](https://slurm.schedmd.com/SUG14/sched_tutorial.pdf)




