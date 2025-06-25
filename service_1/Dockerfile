FROM golang:1.22-alpine

WORKDIR /app

COPY . .

RUN go mod init service1 && go build -o main .

EXPOSE 8001

CMD ["./main"]
