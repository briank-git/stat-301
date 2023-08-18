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
  Rscript -e "remotes::install_github('allisonhorst/palmerpenguins@v0.1.0')" && \
  Rscript -e "install.packages('faux', repos='http://cran.us.r-project.org')" && \
  Rscript -e "install.packages('contextual', repos='http://cran.us.r-project.org')" && \
  Rscript -e "install.packages('ISLR', repos='http://cran.us.r-project.org')" && \
  Rscript -e "install.packages('ISLR2', repos='http://cran.us.r-project.org')" && \
  Rscript -e "install.packages('glmbb', repos='http://cran.us.r-project.org')"

# Install nbgitpuller, jupytext, jlab-git
RUN jupyter labextension install @techrah/text-shortcuts
  
# Install the extension to remove the shift+M merge shortcut
RUN mkdir -p /home/${NB_USER}/.jupyter/lab/user-settings/\@jupyterlab/shortcuts-extension
COPY rm-merge-shortcut.py /home/${NB_USER}/.jupyter/lab/user-settings/\@jupyterlab/shortcuts-extension/shortcuts.jupyterlab-settings
