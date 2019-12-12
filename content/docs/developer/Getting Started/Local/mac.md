---
title: "macOS"
weight: 1
description: >
  A quick start guide on how to run a ConsenSource node locally on macOS for development purposes.
---

## Requirements

The following tools are required to run ConsenSource locally:

- [Docker](https://docs.docker.com/get-started/) - `brew install docker`

If you wish to perform development work, the following additional tools are required:

- [npm](https://docs.npmjs.com/about-npm/) - `brew install node`
- [Rust/Rustup](https://www.rust-lang.org/tools/install)


## How To Run ConsenSource

{{< alert >}}
The ConsenSource application is comprised of a number of Docker images. To run the app locally, we use `docker-compose` to orchestrate the container network.
{{< /alert >}}

1. Clone the [ConsenSource repo](https://github.com/target/consensource)
   - Via HTTPS: `git clone https://github.com/target/consensource.git`
   - Via SSH: `git clone git@github.com:target/consensource.git`
2. `cd consensource`
3. Pull down Docker images from DockerHub
   - `docker-compose pull`(the images are quite long and will take some time to download)
4. Start the network
   - `docker-compose up`

### Available Services

The following services should be available on your local machine at this point:

- ConsenSource Client: http://localhost:8080/
- ConsenSource REST API: http://localhost:9009/
- Sawtooth REST API: http://localhost:8008/
- PostgreSQL Adminer: http://localhost:8081/

You can run the following cURL command to see the contents of the [genesis block](/docs/terminology) that was created on startup:
```
curl -X GET http://localhost:8008/blocks
```

### Log In To A Container

To log in to a running container for any of the services, you can run:
```
docker exec -it <container_name> /bin/bash
```

For example, to log in to the CLI, you can run:
```
docker exec -it consensource-cli /bin/bash
```
