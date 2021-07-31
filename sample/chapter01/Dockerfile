FROM golang:alpine3.14

COPY ./main.go ./


RUN go build -o ./go-app ./main.go

USER nobody

ENTRYPOINT ["./go-app"]