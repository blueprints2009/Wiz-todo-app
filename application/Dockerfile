# Stage 1: Build the binary
FROM --platform=$BUILDPLATFORM golang:1.21-alpine AS build

ARG TARGETOS
ARG TARGETARCH

WORKDIR /go/src/tasky
COPY . .
RUN go mod download

# Cross-compile the binary
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH \
    go build -o /go/src/tasky/tasky

# Stage 2: Final lightweight image
FROM alpine:3.19 AS release

WORKDIR /app

# Copy artifacts from the build stage
COPY --from=build /go/src/tasky/tasky .
COPY --from=build /go/src/tasky/assets ./assets

# Create the required file with your name
# Replace "Your Name" with your actual name
RUN echo "Wizards" > /app/wizexercise.txt

EXPOSE 8080
ENTRYPOINT ["/app/tasky"]