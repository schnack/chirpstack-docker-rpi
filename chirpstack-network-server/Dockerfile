FROM golang:1.15-alpine AS development

ARG VERSION=v3.10.0

ENV PROJECT_PATH=/chirpstack-network-server
ENV PATH=$PATH:$PROJECT_PATH/build
ENV CGO_ENABLED=0
ENV GO_EXTRA_BUILD_ARGS="-a -installsuffix cgo"

RUN apk add --no-cache ca-certificates tzdata make git bash protobuf

RUN git clone --depth 1 --branch $VERSION https://github.com/brocaar/chirpstack-network-server.git $PROJECT_PATH
COPY . $PROJECT_PATH
WORKDIR $PROJECT_PATH

RUN make dev-requirements
RUN make

FROM alpine:3.11.2 AS production

RUN apk --no-cache add ca-certificates tzdata
COPY --from=development /chirpstack-network-server/build/chirpstack-network-server /usr/bin/chirpstack-network-server
ENTRYPOINT ["/usr/bin/chirpstack-network-server"]