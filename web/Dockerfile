FROM golang:1.15.6-alpine3.12 as build_stage
ENV CGO_ENABLED 0

# Copy only the source file. We do not need the "static"
# directory in the build phase.
COPY dispatcher.go .

# Compile the Go program as instructed.
RUN go build dispatcher.go

# Start a new stage.
FROM alpine:3.12

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
