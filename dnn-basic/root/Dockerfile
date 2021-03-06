#
# dnn-basic-root: Basic Dockerfile for deep learning with scikit-image, Lasagne,
# Keras, and ml-pyxis with Jupyter.
#
# This Dockerfile ensures that the container starts as root.
#

# Base image is python-theano-tf
FROM aleksaro/python3-theano-tf:latest


MAINTAINER Aleksander Rognhaugen


# Run the following as `root`
USER root


# Install scikit-image, Lasagne, Keras, and ml-pyxis
RUN pip3 install --upgrade \
    scikit-image \
    git+git://github.com/Lasagne/Lasagne.git@$master \
    git+git://github.com/fchollet/keras.git@$master \
    git+git://github.com/vicolab/ml-pyxis.git@$master


# Set correct locale for locale-aware programs or libraries
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8


# Set up directories in /root
RUN mkdir /root/shared && \
    mkdir /root/data && \
    mkdir /root/.jupyter && \
    mkdir /root/.misc && \
    mkdir -p -m 700 /root/.local/share/jupyter


# Copy over configuration and run file for Jupyter
# The run file is because of: https://github.com/ipython/ipython/issues/7062
COPY jupyter_notebook_config.py /root/.jupyter/
COPY run_jupyter.sh /root/.jupyter/

# Make sure that the run file for jupyter is runnable + add shortcut
RUN chmod +x /root/.jupyter/run_jupyter.sh && \
    echo "alias run_jupyter=/root/.jupyter/run_jupyter.sh" \
    >> /root/.bash_aliases


# Add a script that makes it easier to update some packages
COPY upgrade_packages.sh /root/.misc/
RUN chmod +x /root/.misc/upgrade_packages.sh && \
    echo "alias upgrade_packages=/root/.misc/upgrade_packages.sh" \
    >> /root/.bash_aliases


# Expose port for IPython (8888)
EXPOSE 8888

# Expose port for TensorBoard (6006)
EXPOSE 6006


# Set default working directory and image startup command
WORKDIR "/root"
CMD ["/bin/bash"]
