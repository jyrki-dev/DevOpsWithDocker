FROM alpine:3.16.2

WORKDIR /usr/src/app

RUN apk add curl

COPY script.sh .

RUN chmod +x script.sh

# CMD ./script.sh
ENTRYPOINT [ "./script.sh" ]