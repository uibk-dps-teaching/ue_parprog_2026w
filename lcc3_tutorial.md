# LCC3 Cluster Tutorial

This short tutorial should help you with logging in to and working on the LCC3 cluster at the University of Innsbruck. Please refer to the various links for more information or ask your course instructor in case of problems.

## General Makeup and Usage of Clusters and Supercomputers

Larger clusters and supercomputers work differently than personal computers or laptops. They represent multiple individual computers (called *nodes*) that are connected via a fast network interconnect.

There are two types of nodes, one or a couple of *login node(s)* and a larger number of *compute nodes*. They all usually consist of the same hardware and software stack, but differ in their usage. The login nodes are used for logging in via ssh, compiling, editing scripts or configuration files, etc. Multiple people are working simultaneously on the same login nodes and they are not used for actual computation of your applications. For this purpose, the cluster has compute nodes, which are usually reserved exclusively for running applications.

The process of getting access to a compute node and running your application is handled by a so-called *submission system* which manages all node allocations, scheduling, cleanup, etc. Submission systems are required to allow a large number of scientists to share the same cluster without interference. The user interacts with these submission systems by writing *job scripts*, which tell the submission system what to do for a proper application execution. Hence, the usual working order on a cluster is

1. ssh to the login node of your cluster
2. compile the application and ensure it can be executed properly
3. write a job script
4. submit the job using the job script - the job will be placed in a queue and the submission system decides when it will be executed
5. wait for the job to start, run, and finish and inspect the output
6. depending on the output: either fix any bugs or enjoy the rest of the day

## LCC3 @ UIBK

LCC3 is a cluster of the University of Innsbruck reserved exclusively for teaching purposes. The hardware is a bit dated (it used to be a fast, top-of-the-line scientific cluster in ~2011) but this is irrelevant for our purpose. The basic principles of supercomputers and high performance computing still apply, and the cluster is idle most of the time, ensuring short queuing times.

### General

LCC3 is located on-campus and provides a single login node (`login.lcc3.uibk.ac.at`) and multiple compute nodes (`n000`-`n0XX`). The submission system used by LCC3 is SLURM. Each node consists of two Intel Xeon X5650 six-core processors and 48 GB of main memory. **The system is reachable only from within the UIBK network (requires VPN when working from home).**

More documentation can be found on the ZID website https://www.uibk.ac.at/zid/systeme/hpc-systeme/leo3/.

### Storage

LCC3 has two main storage mount points, `/home/cb76/<username>` (`$HOME`) and `/scratch/<username>` (`$SCRATCH`) , both of which are network-mounted on all login and compute nodes. The former is limited to 1 GB, hence if you run out of storage or start to get weird-looking I/O errors, consider moving your stuff to `/scratch/<username>`, which has a limit of 100 GB.

### Submission System

While you should consult the SLURM documentation (e.g. https://www.uibk.ac.at/zid/systeme/hpc-systeme/common/tutorials/slurm-tutorial.html) for further information, the most important commands are briefly described below:

* `sbatch <jobscript>` - submit a job
* `scancel <job ID>` - delete a job before or during execution
* `squ` - show queued jobs of the cluster
* `srun --pty bash` - login interactively on a compute node (for short debugging sessions only, don't leave this open/idle)

### SLURM Example Job Script for OpenMP applications

While all commands can also be given to `sbatch` on the command line, users usually write job scripts for proper documentation and for keeping the command line invocation as short as possible. The commands in the job script are special comments, marked with `#SBATCH`, which are read and processed by `sbatch`. See [job.sh](job.sh) for an example script.

### Module System

Clusters usually have a lot of software packages installed, often in multiple versions, which might conflict with each other. For this purpose, they use the module system for managing which packages are visible to the user. The most important commands are:

* `module avail` - show available modules that can be loaded
* `module list` - show modules currently loaded
* `module load <module_name>` - load a module
* `module unload <module_name>` - unload a module

### Useful Links

* Current workload of LCC3 (only works within the UIBK network): https://login.lcc3.uibk.ac.at/cgi-bin/state.pl
* Hardware details: https://www.uibk.ac.at/zid/systeme/hpc-systeme/leo3/hardware/

## General hints for ssh usage

### VsCode SSH Remote Extension

- Install `Remote-SSH` extension
- Change VsCode Server install path:
  - Open extension settings
  - Scroll down to `Remote.SSH: Server Install Path`
  - Add Item:
    - Key: `login.lcc3.uibk.ac.at`
    - Item: `/scratch/cbXXXXXX` *(Recommended)*

### SSH setup (Hosts file)

- Edit `~/.ssh/config`
```text
Host lcc3 // Choose whatever name
	HostName login.lcc3.uibk.ac.at
	User cbXXXXXX
```
- Allows use of `ssh lcc3` instead of `ssh cbXXXXXX@login.lcc3.uibk.ac.at`

### Changes for more convenient SSH setup (Keys)

- Re-typing secure passwords is annoying, SSH keys help with this:
```bash
ssh-keygen -t rsa -b 4069 // generate a new keypair using RSA with 4069 bits
```

- You will be prompted on the name/location of the key, default is sufficient
- Next you will be prompted to enter a password for the key (optional)
- Generated keys will be stored in ~/.ssh/, e.g. id_rsa (private) and id_rsa.pub (public)

- Generated SSH keys can be copied to a remote host to be used for login using
```bash
ssh-copy-id cbXXXXXX@login.lcc3.uibk.ac.at
```

- You will be prompted for your cb-accounts password
- On success, the copied keys will be used for authentication on the host

### Editing remote files

- If you are using an editor that does not provide similar funcionality to the `Remote-SSH` extension, editing files on the remote can get tedious
  - Option 1: Edit files locally and copy
    - Copying files is done the easiest using `scp`, which should be included with your distribution
    - `scp ./file/to/copy cbXXXXXX@login.lcc3.uibk.ac.at:/scratch/cbXXXXXX/file/on/remote`
  - Option 2: Mount remote directory using `sshfs`
    - Available in `apt` and `pacman` package managers, `yum` requires install of `epel-release first`
    ```bash
    [pkg-man] [install] sshfs
    // Mounting:
    sshfs lcc3:/remote/dir /local/dir
    sshfs cbXXXXXX@login.lcc3.uibk.ac.at:/remote/dir /local/dir
    // Unmounting:
    fusermount3 -u /local/dir
    ```
