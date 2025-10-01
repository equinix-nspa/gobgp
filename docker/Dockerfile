# Dockerfile for running GoBGP with a config file determine by env variable

# Use a minimal base image, e.g., Alpine Linux
FROM alpine:latest

# Use "--build-arg CONFIG_FILE=<new-value>" to overwrite the default value
ARG CONFIG_FILE="config.ls.bmp.toml"
# Use "--build-arg BRANCH=<new-value>" to overwrite the default value
ARG BRANCH="reshad/bgp-ls-fixes-3.37.0"

# Install necessary packages (e.g., git for cloning GoBGP source, if building from source)
RUN apk add --no-cache git go

# Set the working directory
WORKDIR /app

# Clone the GoBGP repository
RUN git clone https://github.com/equinix-nspa/gobgp.git -b ${BRANCH}

# Build GoBGP from source
WORKDIR /app/gobgp
RUN go install ./cmd/gobgp ./cmd/gobgpd

# Copy the compiled binaries to a more accessible location
RUN cp /root/go/bin/gobgp /usr/local/bin/
RUN cp /root/go/bin/gobgpd /usr/local/bin/

# Expose the BGP port
EXPOSE 179

RUN mkdir /etc/gobgp
COPY ${CONFIG_FILE} /etc/gobgp/gobgp.conf

WORKDIR /go

CMD ["/usr/local/bin/gobgpd", "-f", "/etc/gobgp/gobgp.conf","--log-level=debug", "-p"]
