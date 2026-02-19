# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM

ARG ECOWITT2MQTT_VERSION

ENV LANG=C.UTF-8

# Install and build Ecowitt2MQTT
WORKDIR /usr/src

RUN apt-get update \
    && apt-get install -qy --no-install-recommends \
        adduser \
        python3 \
        build-essential \
        python3-dev \
        libffi-dev \
        python3-aiohttp \
        python3-certifi \
        python3-colorlog \
        python3-fastapi \
        python3-frozenlist \
        python3-meteocalc \
        python3-pydantic \
        python3-python-multipart \
        python3-rapidfuzz \
        python3-ruamel.yaml \
        python3-ruamel.yaml.clib \
        python3-urllib3 \
        python3-uvicorn \
        python3-uvloop \
        python3-paho-mqtt \
        python3-pip \
        git

#RUN addgroup --gid 1000 -S ecowitt2mqtt && adduser -u 1000 -S ecowitt2mqtt -G ecowitt2mqtt
RUN adduser --system --uid 900 --group --no-create-home ecowitt2mqtt

RUN git clone --depth 1 -b "${ECOWITT2MQTT_VERSION}" \
        https://github.com/bachya/ecowitt2mqtt \
    && cd ecowitt2mqtt \
    && sed -e '/^python-multipart/s/0[.]0[.]18/0.0.21/' -i pyproject.toml \
    && pip3 install --no-cache --root-user-action ignore --break-system-packages --compile .
    
# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh

USER 900

CMD [ "/run.sh" ]
