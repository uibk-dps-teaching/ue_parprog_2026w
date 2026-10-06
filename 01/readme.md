# Assignment 1

The goal of this assignment is to get you acquainted with working on a distributed memory cluster as well as obtaining, illustrating, and interpreting measurement data.

## Exercise 1 (1.5 Points)

### Description

This exercise consists in familiarizing yourself with SLURM job submission.

You received user credentials for the LCC3 cluster. If you did not change the default password, do so **immediately**. You are responsible for this account during this semester.

You can find information about LCC3 at https://www.uibk.ac.at/zid/systeme/hpc-systeme/lcc3/ and information about SLURM job submission at https://www.uibk.ac.at/zid/systeme/hpc-systeme/common/tutorials/slurm-tutorial.html.

**Please run any benchmarks or heavy CPU loads only on the compute nodes, not on the login node.**
If you want to do some interactive experimentation, use an *interactive job* as outlined in the tutorial. Make sure to stop any interactive jobs once you are done.

### Tasks

- Study how to submit jobs in SLURM, how to check their state and how to cancel them.
- Prepare a submission script that starts an arbitrary executable, e.g. `/bin/hostname`
- In your opionion, what are the 5 most important parameters available when submitting a job and why? What are possible settings of these parameters, and what effect do they have?
- How do you run your program in parallel? What environment setup is required?

## Exercise 2 (1.5 Points)

### Description

This exercise consists in familiarizing yourself with the hardware of LCC3, that you're running your applications on.

Understanding the architecture of the utilized hardware is paramount for understanding performance and optimizing (parallel) programs. It enables precise resource allocation, memory optimization, and strategic parallelization decisions. In this exercise, we are interested in gathering information about the hardware of the LCC3 compute nodes utilizing the [Portable Hardware Locality (hwloc)](https://www.open-mpi.org/projects/hwloc/) software package.

### Tasks

- Connect to the LCC3 compute cluster using your login credentials. Once connected, load the `hwloc` module via `module load hwloc`.
- Find out more about the hardware that you're running on by executing `lstopo --of ascii`.
- Interpret the output and describe what information you can gather. Please also elaborate on these questions:
  - Can you retrieve the number of CPUs and cores from the output?
  - Investigate the memory hierarchy information provided by lstopo.
    - How much memory (RAM) does the compute node offer?
    - What does the term `NUMANode` tell you about the memory? Why are there two `NUMANodes`?
    - What interesting information can you retrieve about the caches of the system?
  - Is there anything else you can find out from the output?
- Based on your observations, how many threads could you utilize at maximum when parallelizing a program (e.g. with OpenMP) on this system?
- Compare your observations to the [CPU documentation](https://www.intel.com/content/www/us/en/products/sku/47922/intel-xeon-processor-x5650-12m-cache-2-66-ghz-6-40-gts-intel-qpi/specifications.html). Do your observations match the documentation?

## General Notes

All the material required by the tasks above (e.g. code, figures, text, etc...) must be part of the solution that is handed in. Your experiments should be reproducible and comparable to your own measurements using the solution materials that you hand in.

**Every** member of your group must be able to explain the given problem, your solution, and possible findings. You may also need to answer detailed questions about any of these aspects.
