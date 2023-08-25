# Copyright (c) UBC-DSCI Development Team.
# Distributed under the terms of the Modified BSD License.

# TODO: For testing purposes only, change back to ubcdsci/r-stat-301:latest
FROM brianki/r-stat-301-student-test:latest

LABEL maintainer="Brian Kim <brian.kim@stat.ubc.ca>"

# install rise slides extension, nbgrader, clean cache
RUN mamba install --quiet --yes \
    'rise' \
    'nbgrader=0.8.2' && \
    mamba clean --all -f -y
