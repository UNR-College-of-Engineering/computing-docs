# CC petrone group partition

Petrone users have their own node on the College Cluster (CC). The hostname is petrone.engr.unr.edu. You can SSH to it using your netid or start an interactive desktop session using Open OnDemand, which is hosted on the College's head node. The OnDemand URL is https://cc.engr.unr.edu. If you want to use Open OnDemand, you need to supply the following values to the CC Desktop interactive app:

```bash
Number of hours: <Positive Integer - No Limit>

Account: petrone

Partition: petrone

Number of Cores: <Up to 64> # We need to adjust the application to support up to 128.

Enable VirtualGL: Uncheck # node doesn't have an OpenGL capable card.

Email: <Your email>

Extra Slurm Options: <blank>
```

You can also schedule work with SLURM using the sbatch command on the node.

We can not grant sudo privileges to users on the node since this node is accessible via Open OnDemand and integrated with the cluster. By having the node a part of the SLURM cluster, students can tap into additional resources that we are adding to CC in the near future.

---

## TensorRT: 

If the version of TensorRT that is installed is not the version you need, you can pull a container created by NVIDIA for the version you need (https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorrt) with the following commands:
```bash
cd ~/
singularity build --nv --fix-perms tensorrt.22.06-py3.sif  docker://nvcr.io/nvidia/tensorrt:22.06-py3
```

Then you can use the container with the following commands:
```bash
# Add a 4G overlay to the container so it can be writable:
singularity overlay create -s 4096 tensorrt.22.06-py3.sif 
# The container is writable and easy to modify now.

# shell into the container: 
singularity shell --nv --writable tensorrt.22.06-py3.sif

# Run a sample: 
cd /workspace/tensorrt/samples/sampleMNIST
make
/workspace/tensorrt/bin/sample_mnist
```
---

## OpenSees: 

The OpenSees app was installed as a singularity container on the petrone node, you can run the container with:
```bash
singularity shell /opt/singularity/containers/production/opensees-latest-bionic.sif
# at the singularity shell prompt, enter OpenSeesP: 
Singularity> OpenSeesP
```
