# Dockerfile for scientific computing

Dockerfile with all necessary libraries and codes for deploying VASP, triqs, and pyed. (of course no VASP is included here)

Files needed to successfully build this container image:
- mycert.pem and mycery.key for running jupyter with ssh config
- csc_vasp.tar.gz: compressed tar archive containing a vasp version
- jupyter_notebook_config.py file containing your personal notebook configuration
