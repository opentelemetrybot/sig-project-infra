# syntax=docker/dockerfile:1.17@sha256:38387523653efa0039f8e1c89bb74a30504e76ee9f565e25c9a09841f9427b05
FROM golang:1.24.4-bullseye@sha256:dfd72198d14bc22f270c9e000c304a2ffd19f5a5f693fad82643311afdc6b568 as builder

WORKDIR /src
COPY . .
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    cd ./otto && go build -o /out/otto ./cmd/otto

FROM debian:bullseye-slim@sha256:779034981fec838da124ff6ab9211499ba5d4e769dabdfd6c42c6ae2553b9a3b
RUN useradd -m otto
COPY --from=builder /out/otto /usr/local/bin/otto
USER otto
WORKDIR /home/otto

# Expose the service port (default in main.go)
EXPOSE 8080

ENV OTTO_ADDR=:8080

ENTRYPOINT ["/usr/local/bin/otto"]
