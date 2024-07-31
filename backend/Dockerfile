FROM mirror.gcr.io/golang:alpine3.20 as builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY ./cmd/api ./cmd/api
COPY ./internal ./internal
RUN go build -o momo ./cmd/api

# шаг релиза
FROM mirror.gcr.io/golang:alpine3.20
WORKDIR /app
COPY --from=builder /app/momo .
ENTRYPOINT ["./momo"]
EXPOSE 8081
