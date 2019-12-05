---
author: "Samuel Orso, Lionel Voirol"
title: "High Performance Computing - Baobab Hello World"
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
 * As indicated above, to work with Baobab, you will be coding instructions on a LINUX command prompt, typing `bash` commands. We will list in this tutorial the most frequently used `bash` commands when working with Baobab.
 * Regarding the architecture of Baobab, one must understand that Baobab is composed of different partitions, each partition composed of different nodes, for which, each node is composed of a number of CPU/GPU. One can find further details about partitions and their various limits [here](https://baobab.unige.ch/enduser/src/enduser/enduser.html#)
.
 * Baobab schedules tasks using `slurm` cluster management and job scheduling system. 

# Useful `bash` commands

|Command syntax       | Description                  |
|----------------     |  ----------------------------|
|`ls -l`                | list current directory       |
|`pwd`                  | print working directory      |
|`cd ~`                 | navigate to home directory   |
|`cd ..`                | navigate up one directory    |
|`cp oldfile newfile`   | make a copy of a file        |
|`mv oldfile newfile`   | rename a file                |
|`rm file`              | delete a file                |

# Useful `slurm` commands

|Command syntax       | Description                            |
|---------------------| ---------------------------------------|
|`sbatch`               | submit a job script for later execution|
|`scontrol`             | display the slurm state of a given job |
|`scancel`              | cancel a running or pending job        |
|`squeue -u username`   | display pending job of username        |


# Your first `bash` script to execute a `R` script

In order to launch a given Rscript to be executed on Baobab, one need to execute a BASH script via the command `sbatch`. Let's look at an example of a simple BASH script that launch a given `R` script.

```bash
#!/bin/bash
#SBATCH --job-name=simu_R
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=1
#SBATCH --time=0:15:0
#SBATCH --partition=debug-EL7
#SBATCH --mail-type=ALL
#SBATCH --mail-user=firstname.lastname@unige.ch

module load GCC/8.2.0-2.31.1 OpenMPI/3.1.3 R/3.6.0

INFILE=simu.R
OUTFILE=report_simu.Rout

srun R CMD BATCH $INFILE $OUTFILE
```
#### Interpreting the above bash script
 * The first line is called a shebang or hashbang and indicate to the shell what program to interpret the script with.
 * Lines 2 to 8 are `slurm` command options. Here is a table that present these options.

|Command syntax       | Description                                |
|---------------------| ---------------------------------------    |
|`--job-name`           |job name                                    |
|`--ntasks-per-node`    |number of nodes on which to run the job     |
|`--cpus-per-task`      |number of CPU required per task             |
|`--time`               |wall clock time limit                       |
|`--partition`          |partition(s) on which to run the job        |
|`--mail-type`          |select which event types to notify the user |
|`--mail-type`          |user to receive email notification          |

 * line 10 load modules required to run `R`.
 * line 12 and 13 specify both input and output files.
 * line 15 run launch the execution of the task.

Imagine that you create the following `R` script and want to run it on Baobab:

```r
#Load libraries
library(foreach)

#Simulate population
mysd = 15
mymean = 50
pop = rnorm(10e5, mean = mymean, sd = mysd)

#Define nbr of iterations
B = 10e3
samplesize = 100
myresults = foreach(b = icount(B), .combine = rbind)%do%{
  mysample = sample(pop, size = samplesize)
  mean(mysample)
}

#Theoretical xbar variance
mysd^2 / samplesize

#Observed xbar variance
var(myresults[,1])

#Save results
save(myresults, "xbar_simulation_results.rda")

```

In order to transfer it on your LINUX session, you can either write it on your computer and then transfer it on your LINUX session using for example Filezilla, or you can directly write it on your LINUX session using `vim`. The following command will create a `.R` file that you can then edit and save it using `vim` commands.
```bash
vim simu.R
```
Once this `R` script is saved on your LINUX session as `simu.R`, you can then run it on Baobab by running the previously discussed `bash` script. Assuming that you save the above `bash` script as `launch_simu.sh`, you can launch the job with the following command. 

```bash
sbatch launch_simu.sh
```
# Useful ressources

 * [What is Baobab](https://plone.unige.ch/distic/pub/hpc/baobab_en)
  * [Baobab Documentation](https://baobab.unige.ch/enduser/enduser.html)
  * [Slurm Documentation](https://slurm.schedmd.com/)
  * [Slurm Scheduling for Optimal Responsiveness and Utilization](https://slurm.schedmd.com/SUG14/sched_tutorial.pdf)




