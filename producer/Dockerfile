
# STEP 1: use golang image to build an executable which will be used in container
FROM golang:latest AS builder
# Set current directory in container to copy from host
WORKDIR $GOPATH/src/

# copy files from given host directory to current folder
COPY . .

# build current folder and store executable file.
RUN CGO_ENABLED=0 GOOS=linux go build -o /go/bin/app

# STEP 2: build container with executable with scratch as base.
FROM scratch

# copy the ca-certificate.crt from the build stage
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

# Copy our static executable.
COPY --from=builder /go/bin/app /go/bin/app

# set executable as entry point during container startup.
CMD ["/go/bin/app"]
ENTRYPOINT ["/go/bin/app"]