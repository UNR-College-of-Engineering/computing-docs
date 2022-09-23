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
which is hosted on the College's head node. The OnDemand URL is <https://cc.engr.unr.edu>.
If you want to use Open OnDemand, you need to supply the following values to the CC Desktop interactive app:

*Note: OnDemand GUIs will not function until SLURM can be rebooted. We are currently draining the nodes. -ZN 9/1/2021

```text
Number of hours: <Positive Integer - No Limit>
Account: seylabi
Partition: seylabi
Number of Cores: <Up to 128>
Enable VirtualGL: Uncheck #Node doesn't have an OpenGL capable card.
Email: <Your email>
Extra Slurm Options: <blank>
```

You can also schedule work with SLURM using the `sbatch` command on the node.

We can not grant sudo privileges to users on the node since this node is accessible
via Open OnDemand and integrated with the cluster. By having the node a part of the SLURM cluster,
students can tap into additional resources that we are adding to CC in the near future.

Seylabi users home directories are on the cc node.
They are exported as an NFS share to seylabi. The local storage on seylabi is mounted
to `/scratch`.

---

## Seismo VLAB

I created a Singularity definition file that builds a Ubuntu 18.04 container that
installs SVL in `/opt`. The container can be built with `fakeroot` to give you
`root` access inside of the container, which will allow you to install whatever software
you need. You can build a container with the commands below.

```bash
ssh seylabi
cd /scratch #fakeroot can not be built on NFS shares, IE your homedir.
echo $USER #$USER is equal to your netid
singularity build --sandbox -f $USER-svl-bionic svl-bionic.def
```

### Using Seism VLAB

#### Interactive Use

```bash
singularity shell --writable -f /scratch/$USER-svl-bionic
```

You should see the following prompt:

```text
(svl-bionic)root@seylabi:/scratch$
```

From this prompt you can run jobs interactively.

```bash
cd /opt/SVL-1.0-stable/03-Validations/02-Performance
unzip J13-DY_Elastic_3D_Half_Space_PML8_Hexa8.zip
cd J13-DY_Elastic_3D_Half_Space_PML8_Hexa8
#You need to patch the J13 sample for it to run properly on more than 16 cores.
#The MUMPS solver has to be changed to PETSC. The patch sets the number of cores
#to 32.
patch -p1 < /opt/j13.patch
python3 J13-DY_Elastic_3D_Half_Space_PML8_Hexa8.py
mpirun --allow-run-as-root -np 32 /opt/SVL-1.0-stable/02-Run_Process/SeismoVLAB.exe -dir '/opt/SVL-1.0-stable/03-Validations/02-Performance/J13-DY_Elastic_3D_Half_Space_PML8_Hexa8/Partition' -file 'Debugging_J13.1.$.json'
```

#### Batch Use

You have to use the `sbatch` command to queue jobs to execute on the seylabi node.
Below in an example script. `sbatch` allocates the resources `srun` uses the resources.
It's best practice to estimate the amount of memory and time you need for your command(s).

```bash
#!/usr/bin/bash
#SBATCH --job-name=svl-j13-username   # Job name: svl-username-jobid
#SBATCH --mail-type=ALL               # Mail events (NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=YOUR_EMAIL        # Where to send mail
#SBATCH --ntasks=32                   # Request 32 cores
#SBATCH --mem=256G                    # Job memory request
#SBATCH --time=1-00:00:00             # Time limit data-hrs:min:sec
#SBATCH --output=svl_%u_output_%j.log # Standard output and error log
#SBATCH --partition=seylabi           # SLURM partition
#SBATCH --account=deans               # SLURM account

CONTAINER="/scratch/YOUR_USERNAME-svl-bionic"
WORKING_DIR="/opt/SVL-1.0-stable/03-Validations/02-Performance"

#One core to unzip J13-DY_Elastic_3D_Half_Space_PML8_Hexa8.zip
singularity exec -f --writable --pwd ${WORKING_DIR} \
  ${CONTAINER} \
  unzip -fo J13-DY_Elastic_3D_Half_Space_PML8_Hexa8.zip


WORKING_DIR="/opt/SVL-1.0-stable/03-Validations/02-Performance/J13-DY_Elastic_3D_Half_Space_PML8_Hexa8"

#Patch J13
singularity exec -f --writable --pwd ${WORKING_DIR} \
  ${CONTAINER} \
  patch -p1 < /apps/svl/j13.patch

#32 cores to process the .py file. Save the output to parse
output=$(singularity exec -f --writable --pwd ${WORKING_DIR} \
  ${CONTAINER} \
  python3 J13-DY_Elastic_3D_Half_Space_PML8_Hexa8.py)

mpi_cmd=$( echo "${output}" | grep -Po '(mpirun.*)$' | sed 's/mpirun/mpirun --mca btl self,sm --allow-run-as-root/g' | tr -d "'" )

echo $mpi_cmd

#Run the MPI Command
singularity exec -f --writable --pwd ${WORKING_DIR} \
  /${CONTAINER} \
  $mpi_cmd

```

## OpenSees Build

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

## Scheduling Jobs with SLURM

```bash

```