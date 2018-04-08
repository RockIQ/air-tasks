# VERSION 0.0.5-customAF
# AUTHOR: Will Wong
# DESCRIPTION: Docker airflow with ECR registry and DooD (Docker outside of Dkr)
# BUILD: docker build --rm -t wongwill86/air-tasks .
# SOURCE: https://github.com/wongwill86/air-tasks

ARG BASE_DIST=alpine

FROM gcr.io/valid-spark-194220/air-tasks:base-${BASE_DIST}
MAINTAINER rllin

ARG BASE_DIST
ARG BASE_TAG
ARG AIRFLOW_HOME=/usr/local/airflow
ENV AIRFLOW_HOME ${AIRFLOW_HOME}
ARG AIRFLOW_USER=airflow
ENV AIRFLOW_USER ${AIRFLOW_USER}

WORKDIR ${AIRFLOW_HOME}

# prepare airflow user
RUN groupadd -r ${AIRFLOW_USER} \
    && useradd -r -d ${AIRFLOW_HOME} -g ${AIRFLOW_USER} -s /bin/bash ${AIRFLOW_USER} \
    && chown -R ${AIRFLOW_USER}: ${AIRFLOW_HOME} \
    && usermod -aG docker ${AIRFLOW_USER} \
    # unfortunately this is required to update the container docker gid to match the
    # host's gid, we remove this permission from entrypoint-dood.sh script
    && echo "${AIRFLOW_USER} ALL=NOPASSWD: ALL" >> /etc/sudoers

# Copy repo contents into docker image ( see .dockerignore to see skipped folders )
COPY . .

ADD key.json ${AIRFLOW_HOME}/key.json

USER ${AIRFLOW_USER}
WORKDIR ${AIRFLOW_HOME}
ENTRYPOINT ["scripts/entrypoint-dood.sh"]
