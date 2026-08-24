# StyleGAN3 Installation Guide (Linux / WSL)

---

## 0. Install WSL on Drive D

Follow the installation steps from the video below to install WSL on Drive D:

Tutorial: https://www.youtube.com/watch?v=ivF1_aUqSDI

After completing the installation, open WSL and continue with the StyleGAN3 installation steps in the next section.

---

## 1. Clone StyleGAN3

```bash
git clone https://github.com/kulthapon/stylegan3.git
cd stylegan3
```

If you encounter a `Could not resolve host` error, configure the DNS:

```bash
sudo nano /etc/resolv.conf
```

Add:

```text
nameserver 8.8.8.8
nameserver 1.1.1.1
```

Save with `Ctrl+O` → `Enter` → `Ctrl+X`.

---

## 2. Install Miniconda

Download Miniconda:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
```

Install Miniconda:

```bash
bash Miniconda3-py39_4.12.0-Linux-x86_64.sh
```

Add Miniconda to the PATH:

```bash
echo 'export PATH="$HOME/miniconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Verify the installation:

```bash
conda --version
```

---

## 3. Create the Conda Environment

Inside the `stylegan3` directory, create the environment using the provided `environment.yml` file:

```bash
conda env create -f environment.yml
```

Then reload the shell configuration:

```bash
source ~/.bashrc
```

---

## 4. Install G++

Install the required build tools:

```bash
sudo apt update
sudo apt install build-essential
```

Verify the installation:

```bash
g++ --version
```

---

## 5. Install CUDA

Check the NVIDIA driver and CUDA compatibility:

```bash
nvidia-smi
```

For this setup, **CUDA 11.1** is used.

CUDA 11.1 Download Archive:

https://developer.nvidia.com/cuda-11.1.0-download-archive

After installing CUDA, configure `CUDA_HOME`:

```bash
nano ~/.bashrc
```

Add:

```bash
export CUDA_HOME=/usr/local/cuda
export PATH=$CUDA_HOME/bin:$PATH
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH
```

Reload the configuration:

```bash
source ~/.bashrc
```

Verify the configuration:

```bash
echo $CUDA_HOME
nvcc --version
```

---

## 6. Install Additional Dependencies

Install Ninja:

```bash
conda install ninja
```

Initialize Conda:

```bash
conda init
```

Then **close WSL and open it again**.

Navigate back to the StyleGAN3 directory:

```bash
cd stylegan3
```

Activate the environment:

```bash
conda activate stylegan3
```

Install the required packages:

```bash
pip install numpy==1.22.4
pip install psutil
pip install tensorboard
pip install setuptools==65.5.0
pip install distutils
```

---

## 7. Fix `distutils` Error

Open:

```text
~/miniconda3/envs/stylegan3/lib/python3.9/site-packages/torch/utils/tensorboard/__init__.py
```

Replace the relevant section with:

```python
import tensorboard
from setuptools._distutils.version import LooseVersion

if not hasattr(tensorboard, '__version__') or LooseVersion(tensorboard.__version__) < LooseVersion('1.15'):
    raise ImportError('TensorBoard logging requires TensorBoard version 1.15 or above')

from .writer import FileWriter, SummaryWriter  # noqa: F401
from tensorboard.summary.writer.record_writer import RecordWriter  # noqa: F401
```

---

## Notes

* Make sure the **NVIDIA Driver, CUDA, Python, PyTorch, and GPU** versions are compatible.
* For WSL, verify that the NVIDIA GPU is accessible using `nvidia-smi`.
* Run `source ~/.bashrc` after modifying `~/.bashrc`.
* If `conda activate stylegan3` does not work, close and reopen WSL after running `conda init`.
