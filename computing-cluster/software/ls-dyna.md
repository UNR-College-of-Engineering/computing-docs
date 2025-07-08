module purge  
module load openmpi/4.0.5-gcc-10.3.0  
module load ls-dyna/14.0.0/mpp-d-sse2-openmpi  
export LSTC_LICENSE=network  
export LSTC_LICENSE_SERVER=license-0.engr.unr.edu  
mpirun -np {CORECOUNT} ls-dyna i={MODEL.key}  
