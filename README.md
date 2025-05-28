[![arXiv](https://img.shields.io/badge/arXiv-2407.11949-b31b1b.svg)](
https://doi.org/10.48550/arXiv.2407.11949)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.15531613.svg)](https://doi.org/10.5281/zenodo.15531613)

# avqite-z2lgt-avqmetts-paper
Code and data for tensor-network simulations presented in [Minimally Entangled Typical Thermal States for Classical and Quantum Simulation of 1+1-Dimensional $\mathbb{Z}_2$ Lattice Gauge Theory at Finite Temperature and Density](https://arxiv.org/abs/2407.11949). These simulations are performed using [``avqite``](https://github.com/khindanov/avqite) package, which is a tensor-network implementation of the Adaptive Variational Quantum Imaginary-Time Evolution (AVQITE) algorithm.

## Citation
```
@misc{chen2025,
      title={Minimally Entangled Typical Thermal States for Classical and Quantum Simulation of 1+1-Dimensional $\mathbb Z_2$ Lattice Gauge Theory at Finite Temperature and Density}, 
      author={I-Chi Chen and João C. Getelina and Klée Pollock and Aleksei Khindanov and Srimoyee Sen and Yong-Xin Yao and Thomas Iadecola},
      year={2025},
      eprint={2407.11949},
      archivePrefix={arXiv},
      primaryClass={quant-ph},
      url={https://arxiv.org/abs/2407.11949}, 
}
```
## Authors

- **Yongxin Yao** ([@yaoyongxin](https://github.com/yaoyongxin))
- **Aleksei Khindanov** ([@khindanov](https://github.com/khindanov))
- **Thomas Iadecola**

## Repository Description

- ``run.py`` is a runner script to perform simulations using `avqite` package.
- ``incars`` folder contains incar files for all model parameters and seeds used in simulations presented in the paper, as well as the incar-file generating script. Each incar file's name (e.g. `incarL9Zbasis0hz0seed`) is coded in the following way:
    - The system size is $L=9$ qubits (which corresponds to $L=8$ fermions in LGT)
    - The simulation is performed in the $z$-measurement basis for METTS collapse
    - The confining field is $h=0$
    - Random seed is 0th (total 30 seeds are used in the simulations)
- ``data_outs`` folder contains a detailed output log for every simulation. Quanities of interest such as the number of CNOT gates in the final ansatz can be inferred from these output logs.
- ``plt_ncx_tn.ipynb`` is a Jupyter notebook with data analysis and plotting.

## Prerequisites

Before installing Python packages, ensure you have the following prerequisites:

It is recommended to use a **virtual environment**:

1. Using **venv** (built-in):
   ```bash
   python -m venv .venv
   source .venv/bin/activate  
   
   # On Windows: .venv\Scripts\activate
   ```

2. Using **conda**

   - Install Miniconda from the [official website](https://docs.conda.io/en/latest/miniconda.html)

   ```bash
   conda create -n myenv python=3.10
   conda activate myenv
   ```

### MPI (mpi4py) Installation

``avqite`` package uses **MPI (mpi4py)** for parallel execution. See official **mpi4py** [installation page](https://mpi4py.readthedocs.io/en/4.0.3/install.html).


- **Linux/macOS**: It is recommended to use **conda-forge** with the desired MPI implementation, for example MPICH:
  ```bash
  conda install -c conda-forge mpi4py mpich
  ```
  Alternatively, one can try installing via **pip**, but installation problems can be encountered in this case:
  
  ```bash
  python -m pip install mpi4py
  ```

- **Windows**: Install Microsoft MPI (MS-MPI)
  1. Download and install both the MS-MPI SDK and runtime from the [Microsoft website](https://learn.microsoft.com/en-us/message-passing-interface/microsoft-mpi)
  2. Add the MPI installation directory to your system PATH

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/khindanov/avqite-z2lgt-avqmetts-paper.git
   ```

2. Create and activate a new environment (optional)

3. Build ``avqite`` from source (the package has not been yet deployed):
   ```bash
   git clone https://github.com/khindanov/avqite.git
   cd avqite
   pip install -e .
   ```

4. Install **optional dependecies** for ``avqite``:
   ```bash
   # For Windows users 
   pip install .[cotengra_advanced,mpi]

   # For Linux/macOS users
   pip install .[cotengra_advanced,kahypar]

   # For Zsh shell users
   pip install ".[cotengra_advanced,kahypar]"
   ```

5. To use Jupyter notebook, install [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html):
   ```bash
   pip install jupyterlab
   ```

### Important Notes

- Make sure you are in `avqite` directory when building `avqite` package and installing optional dependencies.
- Currently `mpi4py` is required to use `avqite`. However, installing `mpi4py` via `pip install mpi4py` may fail on **macOS**. For this reason `mpi4py` is listed in optional dependencies, and **macOS** users are recommended to install `mpi4py` using `conda-forge` (see above).
- **Windows Users**: The `kahypar` package (which is an optional dependency for `cotengra` used to perform optimized tensor network contractions) is not supported on Windows.

## Running the Code

1. Activate environment:
   ```bash
   conda activate myenv
   ```

2. Execution (here is an example of `incarL9Zbasis0hz0seed` incar file):
    ```bash
    mpiexec -n <number_of_processes> python run.py --filename L9Zbasis0hz0seed --notetras
    ```
