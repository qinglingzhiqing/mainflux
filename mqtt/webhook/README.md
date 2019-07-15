# MQTT Webhook

MQTT webhook provides an HTTP API for MQTT broker (currently VerneMQ and EMQ supported) to call upon
various events (`on_publish()`, `on_subscribe()`, and so on).

## Configuration

The service is configured using the environment variables presented in the
following table. Note that any unset variables will be replaced with their
default values.

| Variable                   | Description                                    | Default               |
|----------------------------|------------------------------------------------|-----------------------|
| MF_WEBOOK_LOG_LEVEL        | Log level for the HTTP Adapter                 | error                 |
| MF_WEBOOK_PORT             | Service HTTP port                              | 8180                  |
| MF_NATS_URL                | NATS instance URL                              | nats://localhost:4222 |
| MF_THINGS_URL              | Things service URL                             | localhost:8181        |
| MF_WEBOOK_CLIENT_TLS       | Flag that indicates if TLS should be turned on | false                 |
| MF_WEBOOK_CA_CERTS         | Path to trusted CAs in PEM format              |                       |

## Deployment

The service is distributed as Docker container. The following snippet provides
a compose file template that can be used to deploy the service container locally:

```yaml
version: "2"
services:
  adapter:
    image: mainflux/http:[version]
    container_name: [instance name]
    ports:
      - [host machine port]:8180
    environment:
      MF_THINGS_URL: [Things service URL]
      MF_NATS_URL: [NATS instance URL]
      MF_WEBOOK_LOG_LEVEL: [HTTP Adapter Log Level]
      MF_WEBOOK_PORT: [Service HTTP port]
```

To start the service outside of the container, execute the following shell script:

```bash
# download the latest version of the service
go get github.com/mainflux/mainflux

cd $GOPATH/src/github.com/mainflux/mainflux

# compile the http
make http

# copy binary to bin
make install

# set the environment variables and run the service
MF_THINGS_URL=[Things service URL] MF_NATS_URL=[NATS instance URL] MF_WEBOOK_LOG_LEVEL=[HTTP Adapter Log Level]
MF_WEBOOK_PORT=[Service HTTP port] $GOBIN/mainflux-http
```

## Usage

For more information about service capabilities and its usage, please check out
the [API documentation](swagger.yaml).