ARG BASE_IMAGE

FROM $BASE_IMAGE

ARG NRFTOOLS_URL
ARG SEGGER_URL
ARG NRF_SDK_URL
ARG NRF_MESH_SDK_URL


RUN apt-get update && \
    apt-get install -y libusb-1.0-0-dev libx11-6 libfreetype6 libxrender1 libfontconfig1 libxext6 xvfb curl unzip python3-pip git zip && \
    rm -rf /var/lib/apt/lists/* && \
    pip install nrfutil

RUN Xvfb :1 -screen 0 1024x768x16 &

ENV INSTALL="/tmp/install"

RUN mkdir -p ${INSTALL} && \
	cd ${INSTALL} && \
	curl -L $NRFTOOLS_URL -o nrftools.zip && \
	unzip nrftools.zip && \
	dpkg -i --force-overwrite JLink_Linux_V758b_x86_64.deb && \
	dpkg -i --force-overwrite nrf-command-line-tools_10.15.4_amd64.deb && \
	cd .. && \
	rm -rf ${INSTALL}

ENV PATH="/opt/mergehex:/opt/nrfjprog:$PATH"

RUN mkdir -p ${INSTALL} && \
	cd ${INSTALL} && \
	curl -L $SEGGER_URL -o ses.tar.gz && \
	tar xf ses.tar.gz && \
	echo "yes" | DISPLAY=:1 $(find arm_segger_* -name "install_segger*") --copy-files-to /ses && \
	cd .. && \
	rm -rf ${INSTALL}

ENV PATH="/ses/bin:${PATH}"

ENV INSTALL="/nordic/sdk"

RUN mkdir -p ${INSTALL} && \
	cd ${INSTALL} && \
	curl -L $NRF_SDK_URL -o nrf_sdk.zip && \
	unzip nrf_sdk.zip && \
	rm nrf_sdk.zip && \
	ln -sf ${INSTALL}(find ${INSTALL} -type d nRF.*) /sdk && \
	cd ..

ENV INSTALL="/nordic/mesh_sdk"

RUN mkdir -p ${INSTALL} && \
	cd ${INSTALL} && \
	curl -L $NRF_MESH_SDK_URL -o nrf_mesh_sdk.zip && \
	unzip nrf_mesh_sdk.zip && \
	rm nrf_mesh_sdk.zip && \
	ln -sf ${INSTALL} /mesh_sdk && \
	cd ..

CMD ["/ses/bin/emBuild"]
