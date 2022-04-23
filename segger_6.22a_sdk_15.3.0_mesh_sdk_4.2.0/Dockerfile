ARG BASE_IMAGE=debian:bullseye-slim

FROM $BASE_IMAGE

ARG NRFTOOLS_URL=https://www.nordicsemi.com/-/media/Software-and-other-downloads/Desktop-software/nRF-command-line-tools/sw/Versions-10-x-x/10-15-4/nrf-command-line-tools-10.15.4_amd64.zip
ARG SEGGER_URL=https://www.segger.com/downloads/embedded-studio/EmbeddedStudio_ARM_Linux_x64
ARG NRF_SDK_URL=https://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v15.x.x/nRF5_SDK_15.3.0_59ac345.zip
ARG NRF_MESH_SDK_URL=https://www.nordicsemi.com/-/media/Software-and-other-downloads/SDKs/nRF5-SDK-for-Mesh/nrf5SDKforMeshv420src.zip


RUN apt-get update && \
	apt-get install -y libusb-1.0-0-dev libx11-6 libfreetype6 libxrender1 libfontconfig1 libxext6 xvfb curl unzip python3-pip git zip && \
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
	curl -L $NRF_SDK_URL -o nRF5_SDK_15.3.0_59ac345.zip && \
	unzip nRF5_SDK_15.3.0_59ac345.zip && \
	rm nRF5_SDK_15.3.0_59ac345.zip && \
	ln -sf ${INSTALL}/nRF5_SDK_15.3.0_59ac345 /sdk && \
	cd ..

ENV INSTALL="/nordic/mesh_sdk"

RUN mkdir -p ${INSTALL} && \
	cd ${INSTALL} && \
	curl -L $NRF_MESH_SDK_URL -o nrf5_SDK_for_Mesh_v4.2.0_src.zip && \
	unzip nrf5_SDK_for_Mesh_v4.2.0_src.zip && \
	rm nrf5_SDK_for_Mesh_v4.2.0_src.zip && \
	ln -sf ${INSTALL} /mesh_sdk && \
	cd ..

CMD ["/ses/bin/emBuild"]
