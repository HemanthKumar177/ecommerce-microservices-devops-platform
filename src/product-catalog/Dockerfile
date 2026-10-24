FROM golang:1.24-alpine AS builder

WORKDIR /usr/src/app

RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

COPY go.mod go.sum ./

COPY . .

RUN go mod download

RUN go build -o product-catalog .

FROM alpine:3.18 AS release

WORKDIR /usr/src/app

COPY ./products ./products/

COPY --from=builder /usr/src/app/product-catalog ./

ENV PRODUCT_CATALOG_PORT=8088

ENTRYPOINT [ "./product-catalog" ]
