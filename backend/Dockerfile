FROM mirror.gcr.io/golang:bullseye as builder
ARG VERSION=${VERSION}
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY ./cmd ./cmd
COPY ./internal ./internal
RUN go build -o momo ./cmd

# шаг релиза
FROM mirror.gcr.io/debian:bullseye-slim
ARG VERSION=${VERSION}
WORKDIR /app
COPY --from=builder /app/momo .
ENTRYPOINT ["./momo"]
EXPOSE 8081