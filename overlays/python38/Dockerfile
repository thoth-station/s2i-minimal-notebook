FROM quay.io/thoth-station/s2i-thoth-ubi8-py38:v0.26.0
LABEL name="s2i-minimal-notebook:latest" \
      summary="Minimal Jupyter Notebook Source-to-Image for Python 3.8 applications." \
      description="Notebook image based on Source-to-Image.These images can be used in OpenDatahub JupterHub." \
      io.k8s.description="Notebook image based on Source-to-Image.These images can be used in OpenDatahub JupterHub." \
      io.k8s.display-name="Minimal Notebook Python 3.8-ubi8 S2I" \
      io.openshift.expose-services="8888:http" \
      io.openshift.tags="python,python38" \
      authoritative-source-url="https://quay.io/thoth-station/s2i-minimal-py38-notebook" \
      io.openshift.s2i.build.commit.ref="master" \
      io.openshift.s2i.build.source-location="https://github/thoth-station/s2i-minimal-notebook" \
      io.openshift.s2i.build.image="quay.io/thoth-station/s2i-thoth-ubi8-py38:v0.26.0"

ENV JUPYTER_ENABLE_LAB="1" \
    ENABLE_MICROPIPENV="1" \
    UPGRADE_PIP_TO_LATEST="1" \
    THAMOS_RUNTIME_ENVIRONMENT="python38" \
    THOTH_ADVISE="0" \
    THOTH_ERROR_FALLBACK="1" \
    THOTH_DRY_RUN="1" \
    THAMOS_DEBUG="0" \
    THAMOS_VERBOSE="1" \
    THOTH_PROVENANCE_CHECK="0"
USER root

# Upgrade NodeJS > 12.0
RUN curl -sL https://rpm.nodesource.com/setup_14.x | bash -  && \
  yum remove -y nodejs && \
  yum install -y nodejs

# Copying in override assemble/run scripts
COPY .s2i/bin /tmp/scripts
# Copying in source code
COPY . /tmp/src
# Change file ownership to the assemble user. Builder image must support chown command.
RUN chown -R 1001:0 /tmp/scripts /tmp/src
USER 1001
RUN /tmp/scripts/assemble
CMD /tmp/scripts/run
