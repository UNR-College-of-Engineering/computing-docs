```bash
module purge
#NOTE: If following lines do not work use this first: module use /apps/modulefiles/tcl
module load openmpi/4.0.5-gcc-10.3.0  
module load ls-dyna/14.0.0/mpp-d-sse2-openmpi  
mpirun -np {CORECOUNT} ls-dyna i={MODEL.key}  
```
