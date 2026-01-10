# Builder Stage
FROM --platform=$BUILDPLATFORM golang:1.25.5-alpine AS builder
ARG TARGETOS
ARG TARGETARCH
WORKDIR /src

COPY go.mod go.sum ./
RUN go mod download
COPY . .

ARG SKAFFOLD_GO_GCFLAGS
RUN GOOS=${TARGETOS} GOARCH=${TARGETARCH} CGO_ENABLED=0 \
    go build -ldflags="-s -w" -gcflags="${SKAFFOLD_GO_GCFLAGS}" -o /productcatalogservice .

# Runtime Stage
FROM gcr.io/distroless/static
WORKDIR /src
COPY --from=builder /productcatalogservice ./server
COPY products.json .
ENV GOTRACEBACK=single
EXPOSE 3550
ENTRYPOINT ["/src/server"]
