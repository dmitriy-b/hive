# This simulation runs the ethereum consensus tests.
FROM golang:1-alpine as builder
RUN apk add --update git ca-certificates gcc musl-dev linux-headers

# Clone the tests repo.
RUN git clone --depth 1 --recurse-submodules https://github.com/ethereum/tests.git /tests && \
    cd /tests && \
    git fetch --depth=1 origin 9201075490807f58811078e9bb5ec895b4ac01a5 && \
    git checkout 9201075490807f58811078e9bb5ec895b4ac01a5 && \
    git submodule update --init --recursive


# Build the simulator executable.
ADD . /source
WORKDIR /source
RUN go build -v .

# Build the simulator run container.
FROM alpine:latest
ADD . /source
WORKDIR /source
COPY --from=builder /source/consensus .
COPY --from=builder /tests /tests
ENV TESTPATH /tests

ENTRYPOINT ["./consensus"]
