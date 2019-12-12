---
title: "Application Developer's Guide"
linkTitle: "Application Developer's Guide"
weight: 4
description: >
  Deeper dives on the individual services that make up the ConsenSource application
---


## Before You Begin

A working knowledge of the following tools will be helpful when running and developing ConsenSource:

- [Hyperledger Sawtooth](https://sawtooth.hyperledger.org/docs/core/releases/1.2/introduction.html)
- [Protocol Buffers (Protobufs)](https://developers.google.com/protocol-buffers/docs/overview)
- [Docker](https://docs.docker.com/get-started/)
- [Rust/Rustup](https://www.rust-lang.org/tools/install)

## Running Local Images

While developing, in order to run your updated code you will need to build and run local docker images rather than the images that have been pulled down from DockerHub. 

### Replace A Remote Image With A Local Image

To replace a Docker image pulled from a remote source with a local build, you can run this command:
```
docker-compose -d --build <service_name>
```

For example, to replace the CLI image, you can run:
```
docker-compose -d --build cli
```

### Replace A Local Image With A Remote Image 

To pull an image from DockerHub back down and replace a local image, you can run:
```
docker-compose pull <service_name>
```

For example, to replace a local CLI image with one from DockerHub, you can run:
```
docker-compose pull cli
```

## Troubleshooting

See the [ConsenSource troubleshooting guide](/docs/troubleshooting/)

## Guides
