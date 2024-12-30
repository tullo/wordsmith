FROM golang:1.17.3-alpine3.13 as build_stage
ENV CGO_ENABLED 0

# Copy only the source file. We do not need the "static"
# directory in the build phase.
COPY dispatcher.go .

# Compile the Go program as instructed.
RUN go build dispatcher.go

# Start a new stage.
FROM alpine:3.21.0

# Copy the binary.
# Note that the golang image uses /go as the WORKDIR.
COPY --from=0 /go/dispatcher .

# Copy the static directory.
COPY static static

# Using the exec syntax (JSON list of strings) makes
# sure that "dispatcher" will be PID 1, and will therefore
# receive signals correctly (allowing Ctrl-C to work
# correctly, for instance).
CMD ["./dispatcher"]
