# scientific software stack with Docker

scientific software stack including major DFT codes, wannier90, triqs, jupyter notebook server, python3-scipy, pandas, latex, etc. all in one container build on top of Ubuntu 20.04. Only add an archive containing vasp.

- `Dockerfile_base` needs to be build first, containing the base installation
- `Dockerfile_dft_stack` uses the base image and builds QE, Vasp, Elk, and wannier90
- `Dockerfile_triqs_stack` uses any of the two above to build a complete triqs environment

