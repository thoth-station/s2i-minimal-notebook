# S2I Minimal Notebook

Minimal Thoth S2I notebook builder

This repository is a Fork of Graham Dumpleton: [jupyter-on-openshift/jupyter-notebooks](https://github.com/jupyter-on-openshift/jupyter-notebooks).

This repository contains Source-to-Image (S2I) build process to create a Minimal Jupyter Notebooks on OpenShift. The image can be built in OpenShift, separately using the `s2i` tool, or using a `docker build`. The same image, can also be used as an S2I builder to create customised Jupyter notebook images with additional Python packages installed, or notebook files preloaded.

## Importing the Minimal Notebook

A pre-built version of the minimal notebook which is based on Thoth Ubi8 Images, can be found at on quay.io at:

- <https://quay.io/repository/thoth-station/s2i-minimal-notebook> [![Docker Repository on Quay](https://quay.io/repository/thoth-station/s2i-minimal-notebook/status "Docker Repository on Quay")](https://quay.io/repository/thoth-station/s2i-minimal-notebook)
- <https://quay.io/repository/thoth-station/s2i-minimal-py38-notebook> [![Docker Repository on Quay](https://quay.io/repository/thoth-station/s2i-minimal-py38-notebook/status "Docker Repository on Quay")](https://quay.io/repository/thoth-station/s2i-minimal-py38-notebook)

This image could be imported into an OpenShift cluster using OpenShift ImageStream:

```yaml
apiVersion: image.openshift.io/v1
kind: ImageStream
metadata:
  # (Below label is needed for Opendatahub.io/JupyterHub)
  # labels:
  #   opendatahub.io/notebook-image: "true"
  name: s2i-minimal-notebook
spec:
  lookupPolicy:
    local: true
  tags:
  - name: latest
    from:
      kind: DockerImage
      name: quay.io/thoth-station/s2i-minimal-notebook:latest
```

## Building the Minimal Notebook

Instead of using the pre-built version of the minimal notebook, you can build the minimal notebook from source code. we follow overlay based method in s2i-minimal-notebook build. A tool Thamos is used for the installation of python stacks.Details about the tool can be found at [Thamos Documentation](https://github.com/thoth-station/thamos#support-for-multiple-runtime-environments)

- Build python36 from the overlay/python36 by setting the environment variable `THAMOS_RUNTIME_ENVIRONMENT=python36`

  ```bash
  s2i build . quay.io/thoth-station/s2i-thoth-ubi8-py36:latest \
  --env ENABLE_PIPENV=1 \
  --env THAMOS_RUNTIME_ENVIRONMENT=python36 \
  --env THOTH_ADVISE=0 \
  --env THOTH_ERROR_FALLBACK=1 \
  --env THOTH_DRY_RUN=1 \
  --env THOTH_PROVENANCE_CHECK=0 \
  s2i-minimal-notebook
  ```

  [Thoth](https://thoth-station.ninja/) advise provides recommendation for python stack directly during the build time.<br>
  It can be used by setting the environment variable THOTH_ADVISE=1

  ```bash
  s2i build . quay.io/thoth-station/s2i-thoth-ubi8-py36:latest \
  --env ENABLE_PIPENV=1 \
  --env THAMOS_RUNTIME_ENVIRONMENT=python36 \
  --env THOTH_ADVISE=1 \
  --env THOTH_DRY_RUN=0 \
  --env THOTH_PROVENANCE_CHECK=1 \
  s2i-minimal-notebook
  ```

- Build python38: From the overlay/python38 by setting the environment variable `THAMOS_RUNTIME_ENVIRONMENT="python38"` in the Dockerfile

  ```bash
  podman build -t s2i-minimal-py38-notebook -f overlays/python38/Dockerfile
  ```
