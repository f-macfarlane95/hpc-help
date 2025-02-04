GPU Processing
==============

The cluster contains three nodes with **Nvidia GPU** co-processors to accelerate CPUs for general-purpose scientific and engineering computing.

- ``jaws`` contains a single Tesla V100 card with 16 GB of dedicated GPU memory
- ``thanos`` contains dual Tesla V100 cards, each with 32 GB of dedicated GPU memory
- ``twiki`` contains dual Quadro RTX 8000 cards, each with 48 GB of dedicated GPU memory

The Tesla cards each have 5,120 **CUDA** processing cores and 640 **Tensor** cores. The RTX cards have 4,608 **CUDA** processing cores and 576 **Tensor** cores. CUDA is Nvidia's parallel computing platform and API for general GPU processing, whereas Tensor cores are specifically intended to speed up the training of neural networks.

More iformation about the cards is available at https://www.nvidia.com/en-gb/data-center/tesla-v100/ and https://www.nvidia.com/en-gb/design-visualization/quadro/rtx-8000/.

To access the GPUs submit to the ``gpu`` Slurm partition, for example, to run an interactive job::

  $ srsh --partition=gpu --gpus=1

.. note::
  Slurm is configured to allocate GPU resources at the level of whole GPUs (rather than CUDA cores), therefore you can request ``--gpus=1`` (all nodes) or ``--gpus=2`` (``thanos`` and ``twiki`` only).

``thanos`` is also a high memory node and spans two partitions, one for high memory jobs and one for GPU jobs. Currently the ``gpu`` partition has ~70 GB of RAM assigned to it and 16 CPUs, therefore the maximum resources you can allocate to a single GPU job are::

  --partition=gpu --mem=66G --cpus-per-task=16 --gpus=2

See also Slurm's documentation on `Generic Resource Scheduling <https://slurm.schedmd.com/gres.html#Running_Jobs>`_.

Whenever you run a job on a GPU node, your path will be modified to include Nvidia's cuda platform. This will be required if compiling any programs from source, for example using the ``nvcc`` compiler. Most of the system's cuda files can be found at ``/usr/local/cuda/bin``.

To see the current state (and power usage) of the GPUs, run::

  $ nvidia-smi

  +-----------------------------------------------------------------------------+
  | NVIDIA-SMI 440.33.01    Driver Version: 440.33.01    CUDA Version: 10.2     |
  |-------------------------------+----------------------+----------------------+
  | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
  | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
  |===============================+======================+======================|
  |   0  Tesla V100-PCIE...  Off  | 00000000:25:00.0 Off |                    0 |
  | N/A   28C    P0    36W / 250W |      0MiB / 32510MiB |      0%      Default |
  +-------------------------------+----------------------+----------------------+
  |   1  Tesla V100-PCIE...  Off  | 00000000:C8:00.0 Off |                    0 |
  | N/A   27C    P0    37W / 250W |      0MiB / 32510MiB |      0%      Default |
  +-------------------------------+----------------------+----------------------+

  +-----------------------------------------------------------------------------+
  | Processes:                                                       GPU Memory |
  |  GPU       PID   Type   Process name                             Usage      |
  |=============================================================================|
  |  No running processes found                                                 |
  +-----------------------------------------------------------------------------+
