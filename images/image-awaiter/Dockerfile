# compile the code to an executable using an intermediary image
FROM golang:1.15-alpine@sha256:d491730a043b10ac51ac7437dc1716a2082349addbf0c6510dc8ad4b8a5ce7c7

# VULN_SCAN_TIME=

RUN mkdir -p /build/
COPY *.mod *.go *.sum /build/
WORKDIR /build
RUN CGO_ENABLED=0 GOOS=linux GOARCH=arm64 go build -ldflags '-w -s' -installsuffix cgo -a -o out/image-awaiter


# present the result within a slimmed image
FROM arm64v8/alpine:3

COPY --from=0 /build/out/image-awaiter /image-awaiter



# To debug / develop this code
# ----------------------------
# 1. Setup a kubectl proxy
# > kubectl proxy --port=8080

# 2. Try the API using the proxy...
# > curl http://localhost:8080/apis/apps/v1/namespaces/<namespace>/demonsets/hook-image-puller

# 3. Try the container using the proxy...
# > docker build --tag <name:tag> .
# > docker run -it --rm --net=host <name:tag> /image-awaiter -debug -namespace <namespace> -daemonset hook-image-puller
