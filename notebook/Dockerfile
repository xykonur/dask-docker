FROM jupyter/base-notebook
ENV NB_USER="root"
ENV NB_UID="0"
ENV NB_GID="0"

USER root
RUN apt-get update && userdel -r jovyan && mkdir -p /home/root \
  && apt-get install -yq --no-install-recommends graphviz git vim \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN conda update conda
RUN conda install --yes \
    python-blosc \
    cytoolz \
    dask==1.1.0 \
    nomkl \
    numpy==1.15.4 \
    pandas==0.23.4 \
    ipywidgets \
    jupyterlab==0.35 \
    && jupyter labextension install @jupyter-widgets/jupyterlab-manager dask-labextension@0.2 \
    && pip install graphviz --no-cache-dir  --no-dependencies \
    && conda clean -tipsy \
    && jupyter lab clean \
    && jlpm cache clean \
    && npm cache clean --force \
    && find /opt/conda/ -type f,l -name '*.a' -delete \
    && find /opt/conda/ -type f,l -name '*.pyc' -delete \
    && find /opt/conda/ -type f,l -name '*.js.map' -delete \
    && find /opt/conda/lib/python*/site-packages/bokeh/server/static -type f,l -name '*.js' -not -name '*.min.js' -delete \
    && rm -rf /opt/conda/pkgs

COPY prepare.sh /usr/bin/prepare.sh
RUN chmod +x /usr/bin/prepare.sh
RUN mkdir /opt/app
COPY examples/ /home/$NB_USER/examples
RUN chown -R $NB_USER /home/$NB_USER/examples

ENTRYPOINT ["tini", "--", "/usr/bin/prepare.sh"]
CMD ["start.sh", "jupyter", "lab", "--allow-root"]
