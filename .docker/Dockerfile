ARG BASE_CONTAINER=jupyter/base-notebook:python-3.7.6
FROM $BASE_CONTAINER

LABEL maintainer="Data Lab Project <datalab@noao.edu>"

ENV JUPYTER_ENABLE_LAB='true'

USER root

# install stack
RUN apt-get update && apt-get install -y curl nodejs ffmpeg
RUN conda install -c astropy photutils astroML
RUN pip install healpy

RUN conda install -c conda-forge widgetsnbextension ipywidgets ipympl
RUN jupyter labextension install @jupyter-widgets/jupyterlab-manager

RUN jupyter labextension install jupyter-matplotlib

# install datalab clients and CLI
RUN python -m pip install noaodatalab

RUN /usr/local/bin/fix-permissions $HOME

USER jovyan
WORKDIR $HOME

COPY jupyter_notebook_config.json /opt/conda/etc/jupyter/jupyter_notebook_config.json

# get notebooks-latest
ARG NBTARGET=$HOME/datalab-notebooks/
RUN echo "Fetching Data Lab default notebooks" &&\
    mkdir $NBTARGET

RUN curl -LJ https://github.com/noaodatalab/notebooks-latest/tarball/master | tar xz --strip=1 -C $NBTARGET

WORKDIR $HOME
RUN rmdir $HOME/work/

ENTRYPOINT ["bash","/usr/local/bin/start-notebook.sh"]
