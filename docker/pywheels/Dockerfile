FROM python:alpine3.10

ENV PYWHEELS_DOCKER_GROUP="pywheels" \
    PYWHEELS_DOCKER_USER="pywheels" \
    PYWHEELS_DB_USER="pywheels" \
    PYWHEELS_OUTPUT_PATH="/var/www/pywheels" \
    WORKDIR=/pywheels \
    PATH="/home/pywheels/.local/bin:$PATH" \
    DB_HOST="db" \
    DB_PORT="5432" \
    DB_USER="postgres" \
    DB_PASSWORD="postgres" \
    DB_NAME="postgres"

RUN addgroup -S ${PYWHEELS_DOCKER_GROUP} && \
    adduser  -S ${PYWHEELS_DOCKER_USER} -G ${PYWHEELS_DOCKER_GROUP} && \
    apk add --no-cache \
        bash \
        gcc \
        g++ \
        musl-dev \
        zeromq-dev \
        postgresql-dev && \
    mkdir -p "${PYWHEELS_OUTPUT_PATH}" && \
    chown "${PYWHEELS_DOCKER_GROUP}":"${PYWHEELS_DOCKER_USER}" "${PYWHEELS_OUTPUT_PATH}"

USER ${PYWHEELS_DOCKER_USER}

COPY . ${WORKDIR}

WORKDIR ${WORKDIR}

SHELL ["/bin/bash", "-c"]

ARG PYWHEELS_SERVICE_TYPE

RUN pip install packaging && \
    pip install . && \
    pip install .[${PYWHEELS_SERVICE_TYPE}]

COPY --chown=${PYWHEELS_DOCKER_GROUP}:${PYWHEELS_DOCKER_USER} ./docker/pywheels/start /start
RUN chmod +x /start

CMD ["/start"]