FROM alpine:latest AS build

ARG KUSTOMIZE_VERSION

RUN apk add --no-cache wget tar
RUN wget -nv https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2F${KUSTOMIZE_VERSION}/kustomize_${KUSTOMIZE_VERSION}_linux_amd64.tar.gz && \
  tar xvzf kustomize_${KUSTOMIZE_VERSION}_linux_amd64.tar.gz

FROM gcr.io/google.com/cloudsdktool/cloud-sdk

COPY --from=build kustomize /usr/bin/kustomize

RUN mkdir -p /builder/bin && \
  curl -sSLo /builder/bin/skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 && \
  chmod +x /builder/bin/skaffold 

ENV PATH=/builder/bin/:$PATH
ENV DOCKER_CONFIG=/builder/home/.docker

COPY skaffold.bash /builder/skaffold.bash
RUN chmod +700 /builder/skaffold.bash
ENTRYPOINT ["/builder/skaffold.bash"]
