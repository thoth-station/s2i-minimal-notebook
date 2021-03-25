FROM quay.io/thoth-station/s2i-thoth-ubi8-py36:v0.26.0
LABEL "name"="s2i-minimal-notebook:latest" \
      "summary"="Minimal S2I Jupyter Notebook." \
      "io.openshift.s2i.build.commit.ref"="master" \
      "io.openshift.s2i.build.source-location"="https://github/thoth-station/s2i-minimal-notebook" \
      "io.openshift.s2i.build.image"="quay.io/thoth-station/s2i-thoth-ubi8-py36:v0.26.0"

ENV ENABLE_MICROPIPENV="1" \
    THAMOS_RUNTIME_ENVIRONMENT="python36" \
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
