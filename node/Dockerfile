FROM node:6.11.2-alpine

RUN apk add --no-cache python py-requests py-pip && \
    pip install twilio==5.7.0

WORKDIR /usr/src/app

CMD ["node"]
