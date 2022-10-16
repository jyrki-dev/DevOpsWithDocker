FROM golang:1.16

EXPOSE 8080

WORKDIR /usr/src/app

COPY . .

RUN go build
ENV PORT=8080
ENV REQUEST_ORIGIN=https://example.com

RUN chmod +x server

# ENTRYPOINT /usr/src/app/server
CMD /usr/src/app/server
