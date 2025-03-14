FROM golang:1.22-alpine AS builder

RUN apk add --no-cache gcc musl-dev

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main main.go

FROM alpine:latest

RUN apk --no-cache add ca-certificates

WORKDIR /app

COPY --from=builder /app/main .
COPY --from=builder /app/config ./config
COPY --from=builder /app/db/migrations ./db/migrations

# default ENVs 
ENV BANK_POSTGRES_HOST=localhost:5432
ENV BANK_POSTGRES_PORT=5432
ENV BANK_POSTGRES_USER=test
ENV BANK_POSTGRES_PASSWORD=test
ENV BANK_POSTGRES_DB=bdb
ENV BANK_POSTGRES_SSLMODE=disable

EXPOSE 8080

CMD ["./main"]
