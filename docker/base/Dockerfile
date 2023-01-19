ARG BASE_IMAGE=dustynv/ros:galactic-pytorch-l4t-r32.4.4
FROM ${BASE_IMAGE}

ENV DEBIAN_FRONTEND=noninteractive


# ============================
# INSTALL MISC DEPENDENCIES
# ============================
RUN echo "\e[42m Install misc utils and dependencies \e[0m" && \
    apt-get update && \
    apt-get install -y supervisor unzip net-tools apt-utils && \
    apt-get install -y python3-smbus && \
    pip3 install pyzmq


# =================
# INSTALL GSTREAMER
# =================
RUN echo "\e[42m Install GStreamer \e[0m" && \
    apt-get update && \
    apt-get install -y \
    libwayland-egl1 \
    gstreamer1.0-plugins-bad \
    libgstreamer-plugins-bad1.0-0 \
    gstreamer1.0-plugins-good \
    python3-gst-1.0


# =================
# INSTALL TORCH2TRT
# =================
ENV TORCH2TRT_REPO_DIR=/opt/
RUN echo "\e[42m Install torch2trt \e[0m" && \
    cd ${TORCH2TRT_REPO_DIR} && \
    git clone https://github.com/NVIDIA-AI-IOT/torch2trt && \
    cd torch2trt && \
    python3 setup.py install


# ===================
# INSTALL JUPYTER LAB
# 
# and related stuff:
#    * NodeJS (for jupyterlab-manager)
#    * traitlets (master, to support the unlink() method)
#    * jupyterlab-manager
#    * jupyter_clickable_image_widget
#    * ssh for connecting to host
#====================
RUN echo "\e[42m Install Jupyter Lab prerequicities \e[0m" && \
    apt-get install libffi-dev -y && \
    pip3 install --upgrade requests && \
    \
    echo "\e[42m Install NodeJS \e[0m" && \
    apt-get update && \
    apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs && \
    \
    echo "\e[42m Install Jupyter Lab \e[0m" && \
    pip3 install --no-cache-dir --verbose jupyter jupyterlab && \
    pip3 install --no-cache-dir --verbose jupyterlab_widgets && \
    \
    echo "\e[42m Install jupyterlab-manager \e[0m" && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager && \
    \
    echo "\e[42m Install jupyter_clickable_image_widget \e[0m" && \
    cd && \
    apt-get install -y libssl1.0-dev && \
    git clone https://github.com/jaybdub/jupyter_clickable_image_widget && \
    cd jupyter_clickable_image_widget && \
    git checkout tags/v0.1 && \
    pip3 install -e . && \
    jupyter labextension install js && \
    jupyter lab build && \
    \
    echo "\e[42m Install ssh \e[0m" && \
    apt-get update && \
    apt-get install -y ssh

    # echo "\e[42m Install NodeJS \e[0m" && \
    # apt-get update && \
    # apt-get install -y curl && \
    # curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    # apt-get install -y nodejs && \
    # \
    # echo "\e[42m Install traitlets \e[0m" && \
    # python3 -m pip install git+https://github.com/ipython/traitlets@dead2b8cdde5913572254cf6dc70b5a6065b86f8 && \
    # \


# ==============
# INSTALL JETBOT
# ==============
ENV JETBOT_REPO_DIR=/opt/jetbot
COPY . ${JETBOT_REPO_DIR}
RUN echo "\e[42m Install Jetbot \e[0m" && \
    cd ${JETBOT_REPO_DIR} && \
    python3 setup.py install

