ARG BASE_IMAGE=jetbot-models:jp44
FROM ${BASE_IMAGE}

ARG JUPYTER_PASSWORD=jetbot
ENV JUPYTER_WORKDIR=/workspace
ENV SHELL /bin/bash
WORKDIR /workspace

# ====================
# SET JUPYTER PASSWORD
# ====================
ENV JUPYTER_PASSWORD=${JUPYTER_PASSWORD}
RUN jupyter lab --generate-config -y
RUN python3 -c "from notebook.auth.security import set_password; set_password('${JUPYTER_PASSWORD}', '/root/.jupyter/jupyter_notebook_config.json')"    

CMD cd $JUPYTER_WORKDIR && \
    /bin/bash -c "jupyter lab --ip 0.0.0.0 --port 8888 --allow-root &> /var/log/jupyter.log" & \
	echo "allow 10 sec for JupyterLab to start @ http://$(hostname -I | cut -d' ' -f1):8888" && \
	echo "JupterLab logging location:  /var/log/jupyter.log  (inside the container)" && \
	/bin/bash
