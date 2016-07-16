# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM jupyter/scipy-notebook

MAINTAINER Jupyter Project <jupyter@googlegroups.com>

USER root

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Julia dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    libnettle4 && apt-get clean && \
    rm -rf /var/lib/apt/lists/*



USER $NB_USER

# Install IJulia packages as jovyan and then move the kernelspec out
# to the system share location. Avoids problems with runtime UID change not
# taking effect properly on the .local folder in the jovyan home dir.
RUN julia -e 'Pkg.add("IJulia")' && \
    mv /home/$NB_USER/.local/share/jupyter/kernels/julia* $CONDA_DIR/share/jupyter/kernels/ && \
    chmod -R go+rx $CONDA_DIR/share/jupyter

# Show Julia where conda libraries are
# Add essential packages
RUN echo "push!(Sys.DL_LOAD_PATH, \"$CONDA_DIR/lib\")" > /home/$NB_USER/.juliarc.jl && \
    julia -e 'Pkg.add("Gadfly")' && julia -e 'Pkg.add("RDatasets")' && julia -F -e 'Pkg.add("HDF5")'
