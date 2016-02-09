# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.
FROM debian:latest
MAINTAINER Jeff Smith <dockerjeff@unbiasedgeek.com>

# Make sure the package repository is up to date.
RUN apt-get -y update && \
    apt-get -y install --no-install-recommends \
        ca-certificates \
        curl \
	g++ \
	gcc \
        git \
	libc6-dev \
	make \
	openssh-server && \
    rm -rf /var/lib/apt/lists/*

# Create the gotest user
RUN useradd -u 1000 -m -s /bin/bash gotest

ENV GOPATH=/goapp
ENV GOLANG_VERSION 1.5.3
ENV GOLANG_DOWNLOAD_URL https://golang.org/dl/go$GOLANG_VERSION.linux-amd64.tar.gz
ENV GOLANG_DOWNLOAD_SHA256 43afe0c5017e502630b1aea4d44b8a7f059bf60d7f29dfd58db454d4e4e0ae53

RUN echo "gotest:gotest" | chpasswd && \
	echo "export GOPATH=$GOPATH" >> /home/gotest/.bashrc && \
	echo "export GOLANG_VERSION=$GOLANG_VERSION" >> /home/gotest/.bashrc && \
	echo "export GOLANG_DOWNLOAD_URL=$GOLANG_DOWNLOAD_URL" >> /home/gotest/.bashrc && \
	echo "export GOLANG_DOWNLOAD_SHA256=GOLANG_DOWNLOAD_SHA256" >> /home/gotest/.bashrc && \
	echo "export PATH=$GOPATH/bin:/usr/local/go/bin:\$PATH" >> /home/gotest/.bashrc

RUN curl -fsSL "$GOLANG_DOWNLOAD_URL" -o golang.tar.gz \
	&& echo "$GOLANG_DOWNLOAD_SHA256  golang.tar.gz" | sha256sum -c - \
	&& tar -C /usr/local -xzf golang.tar.gz \
	&& rm golang.tar.gz

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" "$GOPATH/pkg" && chmod -R 777 "$GOPATH"

RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd
RUN mkdir -p /var/run/sshd

# Standard SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]





