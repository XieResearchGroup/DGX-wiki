# DGX wiki

## Wiki for using DGX station
Website: http://dgx-wiki.readthedocs.io/

## For tensorflow users

- Specify the gpu you want to use for each job after checking gpu-usage with **nvidia-smi**
- **Do not** use **allow_soft_placement=True** in your session config to avoid the job is allocated to other gpus other than the one you want to use
- **Make Sure** use option **config.gpu_options.allow_growth = True** in your session config, so that the gpu memory can be released after your session closed. (It seems tensorflow or CUDA don't release gpu memory after session close https://github.com/tensorflow/tensorflow/issues/19731)
