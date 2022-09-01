# CC seylabi group partition

**To connect to the seylabi node, connect to cc-head.engr.unr.edu first, then ssh to seylabi**
```bash
ssh $netid@cc-head.engr.unr.edu
ssh seylabi
```

To enable passwordless SSH from cc-head to seylabi, run the following commands:
```bash
mkdir ~/.ssh
chmod 700 ~/.ssh
ssh-keygen -t ed25519 -N "" -C "cc" -f /home/${USER}/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub > ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```


Seylabi users have their own node on the College Cluster (CC). The hostname is seylabi.engr.unr.edu. 
You can SSH to it using your netid or start an interactive desktop session using Open OnDemand, 
which is hosted on the College's head node. The OnDemand URL is https://cc.engr.unr.edu. 
If you want to use Open OnDemand, you need to supply the following values to the CC Desktop interactive app:

*Note: OnDemand GUIs will not function until SLURM can be rebooted. We are currently draining the nodes. -ZN 9/1/2021

```bash
Number of hours: <Positive Integer - No Limit>

Account: seylabi

Partition: seylabi

Number of Cores: <Up to 128>

Enable VirtualGL: Uncheck #Node doesn't have an OpenGL capable card.

Email: <Your email>

Extra Slurm Options: <blank>
```

You can also schedule work with SLURM using the sbatch command on the node.

We can not grant sudo privileges to users on the node since this node is accessible 
via Open OnDemand and integrated with the cluster. By having the node a part of the SLURM cluster,
students can tap into additional resources that we are adding to CC in the near future.

Seylabi users home directories are on the cc node. 
They are exported as an nfs share to Seylabi. The local storage on Seylabi is not currently
in use and can be used for as a local scratch if necessary.  

---

## Seismo VLAB: 
I have already compiled a container with Seismo VLAB in it. You can use it with Singularity:
```bash
singularity exec /data/containers/prod/svl-ubuntu18.04.simg ls /opt/SVL-1.0-stable/02-Run_Process
```

```text
01-Node       04-Elements  07-Assembler   10-Integrators  Doxyfile  Makefile    *SeismoVLAB.exe*
02-Materials  05-Loads     08-Analysis    11-Solvers      main.cpp  Makefile.mk
03-Sections   06-Mesh      09-Algorithms  12-Utilities    main.o    README.md
```

---

## OpenSees Build:

```bash
module load mpi
cd ~/
git clone https://github.com/OpenSees/OpenSees.git
cd OpenSees/
git checkout tags/v3.4.0
mkdir build
cd build/
cmake ../
cmake --build . -j 64
```
