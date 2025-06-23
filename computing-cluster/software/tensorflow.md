### setting up environment
```bash
module purge
module load python/3.8.12-gcc-10.3.0
module load nvhpc/22.7.cuda10
module load cuda/11.4.2-gcc-10.3.0
module load cudnn/8.6.0
module load tensorrt/8.4.3.1

python3 -m venv <virtual_environment_name>
source <virtual_environment_name>/bin/activate
```

### testing gpu visibility
```bash
import tensorflow as tf
print("Num GPUs Available: ", len(tf.config.list_physical_devices('GPU')))
```
