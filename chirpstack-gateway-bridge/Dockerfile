FROM golang:1.15-alpine AS development

ARG VERSION=v3.9.2

ENV PROJECT_PATH=/chirpstack-gateway-bridge
ENV PATH=$PATH:$PROJECT_PATH/build
ENV CGO_ENABLED=0
ENV GO_EXTRA_BUILD_ARGS="-a -installsuffix cgo"

RUN apk add --no-cache ca-certificates make git bash

RUN git clone --depth 1 --branch $VERSION https://github.com/brocaar/chirpstack-gateway-bridge.git $PROJECT_PATH
COPY . $PROJECT_PATH
WORKDIR $PROJECT_PATH

RUN make dev-requirements
RUN make

FROM alpine:3.11.2 AS production

RUN apk --no-cache add ca-certificates
COPY --from=development /chirpstack-gateway-bridge/build/chirpstack-gateway-bridge /usr/bin/chirpstack-gateway-bridge
ENTRYPOINT ["/usr/bin/chirpstack-gateway-bridge"]