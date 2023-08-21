# Copyright (c) UBC-DSCI Development Team. 
# Distributed under the terms of the Modified BSD License.
FROM jupyter/minimal-notebook

LABEL maintainer="Brian Kim <brian.kim@stat.ubc.ca>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# R pre-requisites
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    fonts-dejavu \
    unixodbc \
    unixodbc-dev \
    r-cran-rodbc \
    gfortran \
    gcc \
    vim && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

WORKDIR "${HOME}"

# remove the "work/" directory added in an earlier layer...
RUN rm -rf work

COPY conda-linux-64.lock /tmp/conda-linux-64.lock

RUN mamba update --quiet --file /tmp/conda-linux-64.lock && \
  mamba clean --all -y -f && \
  fix-permissions "${CONDA_DIR}" && \
  fix-permissions "/home/${NB_USER}" && \
  Rscript -e "install.packages('faux', repos='http://cran.us.r-project.org')" && \
  Rscript -e "install.packages('contextual', repos='http://cran.us.r-project.org')" 

# Configure shortcuts-extension to remove Shift-M cell merge shortcut
COPY shortcuts.jupyterlab-settings /home/${NB_USER}/.jupyter/lab/user-settings/\@jupyterlab/shortcuts-extension/shortcuts.jupyterlab-settings

# Make sure everything in the home folder is owned by NB_USER
USER root
RUN chown -R ${NB_USER} /home/${NB_USER}
USER ${NB_UID}
