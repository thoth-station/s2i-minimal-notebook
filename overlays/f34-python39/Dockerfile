FROM quay.io/thoth-station/s2i-thoth-f34-py39:v0.35.1

ENV IMAGE_NAME="quay.io/thoth-station/s2i-minimal-f34-py39-notebook" \
    IMAGE_TAG="latest" \
    SUMMARY="Minimal Jupyter Notebook Source-to-Image for Python 3.9 applications." \
    DESCRIPTION="Notebook image based on Source-to-Image.These images can be used in OpenDatahub JupterHub." \
    JUPYTER_ENABLE_LAB="1" \
    ENABLE_MICROPIPENV="1" \
    THAMOS_RUNTIME_ENVIRONMENT="f34-python39" \
    THOTH_ADVISE="0" \
    THOTH_ERROR_FALLBACK="1" \
    THOTH_DRY_RUN="1" \
    THAMOS_DEBUG="0" \
    THAMOS_VERBOSE="1" \
    THOTH_PROVENANCE_CHECK="0"

LABEL name="$IMAGE_NAME" \
      version="$IMAGE_TAG" \
      summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Minimal Notebook Python 3.9-f34 S2I" \
      io.openshift.expose-services="8888:http" \
      io.openshift.tags="python,python39" \
      authoritative-source-url="https://quay.io/thoth-station/s2i-minimal-f34-py39-notebook" \
      io.openshift.s2i.build.commit.ref="master" \
      io.openshift.s2i.build.source-location="https://github/thoth-station/s2i-minimal-notebook" \
      io.openshift.s2i.build.image="quay.io/thoth-station/s2i-thoth-f34-py39"

USER root

# clean up cache
RUN rm -rf /var/cache/dnf
# Copying in override assemble/run scripts
COPY .s2i/bin /tmp/scripts
# Copying in source code
COPY . /tmp/src
# Change file ownership to the assemble user. Builder image must support chown command.
RUN chown -R 1001:0 /tmp/scripts /tmp/src
USER 1001
RUN /tmp/scripts/assemble
CMD /tmp/scripts/run
