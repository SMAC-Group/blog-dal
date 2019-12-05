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
However, the documentation already provided can be challenging to grasp for a new user. The aim of this tutorial is therefore to provide a clear and concise introduction to the use of the HPC Cluster Baobab.

# Parallelisation and optimal scheduling
The `bash` script discussed in the previous tutorial "Baobab Hello World" ([here](https://blog-dal.netlify.com/posts/baobab_hello_world/)) run the `R` script written below on one CPU in one node. There is therefore no parallelisation of the code. However, when each iteration on a given programm are independant, one can parallelise the code such that the run time is reduced. We will in this tutorial discuss both the notion of command parallelisation and parallelisation via `slurm`, how one should modify both scripts based on the type of parallelisation and how the code will be executed on Baobab.

## Command parallelisation
One can parallelise his code by distributing calculations of independant iterations over `n` CPU in the same node. This is called command parallelisation. In order to do such, one must change both the `bash` and the `R` code. With reference to the codes presented in the tutorial "Baobab Hello World", one must change both codes as such to parallelise the `R` code on `n` CPU. We will here consider `n` as 8. The `bash` code would therefore be written as such:

```bash
#!/bin/bash
#SBATCH --job-name=simu_R
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=8
#SBATCH --time=0:15:0
#SBATCH --partition=debug-EL7
#SBATCH --mail-type=ALL
#SBATCH --mail-user=firstname.lastname@unige.ch

module load GCC/8.2.0-2.31.1 OpenMPI/3.1.3 R/3.6.0

INFILE=simu.R
OUTFILE=report_simu.Rout

srun R CMD BATCH $INFILE $OUTFILE
```
The `R` code would be written as such, using the `%dopar%` operator instead of `%do%"` and importing library `doParallel`:

```r
#Load libraries
library(foreach)
library(doParallel)

# set the number of cores in the sbatch script
registerDoParallel(cores=Sys.getenv("SLURM_CPUS_PER_TASK"))

# print the number of workers
getDoParWorkers()

#Simulate population
mysd = 15
mymean = 50
pop = rnorm(10e5, mean = mymean, sd = mysd)

#Define nbr of iterations
B = 10e3
samplesize = 100
myresults = foreach(b = icount(B), .combine = rbind)%dopar%{
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

## Notes on optimal scheduling with `slurm`

 * Define `--time` at minimum considering the time required by your task. One can run a small number of iterations of a given task to get an estimate of the time required to run a simulation. 
 * You can list partitions in the parameter `--partition` such that your task has a higher probability of being executed earlier (i.e. `#SBATCH --partition=mono-EL7,parallel-EL7,bigmem-EL7,shared-EL7,mono-shared-EL7,shared-bigmem-EL7`).
 * Note that by specifying such a combination of `--ntasks-per-node` and `--cpus-per-task` your task will be executed only when an available node with that ammount of CPU is available (as oposed to parallelisation via `slurm`).
 * Be sure to specifiy the correct possible `--partition` considering the `--time` parameter indicated and the limitations of each partition. One can find Baobab partitions and limits documentation [here](https://baobab.unige.ch/enduser/src/enduser/enduser.html).
 
## Parallelisation via `slurm`

To be done

# Useful ressources

 * [What is Baobab](https://plone.unige.ch/distic/pub/hpc/baobab_en)
 * [Baobab Documentation](https://baobab.unige.ch/enduser/enduser.html)
 * [Slurm Documentation](https://slurm.schedmd.com/)
 * [Slurm Scheduling for Optimal Responsiveness and Utilization](https://slurm.schedmd.com/SUG14/sched_tutorial.pdf)
