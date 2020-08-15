# Triqs 3 incl. apps

triqs 3 Dockerfile build with:
* Ubuntu 20.04 focal
* clang 9
* OpenMPI
* Intel MKL 2020
* including: DFTTools, cthyb, SoliDMFT
* user ID mapping
* useful tools like VIM, nano, rsync etc.

## how to build:

Just run 
```
docker build -t triqs3-dev ./ 
```
in this directory.
