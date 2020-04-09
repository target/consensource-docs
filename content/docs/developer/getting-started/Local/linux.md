---
title: "Linux"
weight: 3
description: >
  A quick start guide on how to run a ConsenSource node locally on Linux for development purposes.
---

## How To Run ConsenSource

{{< alert >}}
The ConsenSource application is comprised of a number of Docker images. To run the app locally, we use `docker-compose` to orchestrate the container network.
{{< /alert >}}

TODO

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
