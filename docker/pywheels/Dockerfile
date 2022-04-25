FROM python:alpine3.10

ARG PYWHEELS_SERVICE_TYPE

ENV PYWHEELS_DOCKER_GROUP="pywheels" \
    PYWHEELS_DOCKER_USER="pywheels" \
    WORKDIR=/pywheels \
    PATH="/home/pywheels/.local/bin:$PATH"

RUN addgroup -S ${PYWHEELS_DOCKER_GROUP} && \
    adduser  -S ${PYWHEELS_DOCKER_USER} -G ${PYWHEELS_DOCKER_GROUP} && \
    apk add --no-cache \
        bash \
        gcc \
        g++ \
        musl-dev \
        zeromq-dev \
        postgresql-dev

USER ${PYWHEELS_DOCKER_USER}

COPY . ${WORKDIR}

WORKDIR ${WORKDIR}

RUN pip install packaging && \
    pip install . && \
    pip install .[${PYWHEELS_SERVICE_TYPE}]

COPY --chown=${PYWHEELS_DOCKER_GROUP}:${PYWHEELS_DOCKER_USER} ./docker/pywheels/start /start
RUN chmod +x /start

CMD ["/start"]