# Object Storage Python SDK

## Overview

Python can also be used to access the CWM MinIO instances. The Python
SDK provides all the APIs to perform operations similar to MinIO Client (`mc`).

For more details on the MinIO Client, please see the [CLI](cli.md)
documentation.

## Default Ports

The default ports for a CWM MinIO instance are:

- HTTP: 80
- HTTPS: 443

## Endpoint Prefix

The endpoint listed for a CWM MinIO instance i.e. **ACCESS API URL** on the
WebUI may contain the protocol prefix e.g. `https://`. You do not need the
prefix with the JavaScript API to connect to the CWM MinIO instance.

You can consult the
[API reference](https://docs.min.io/docs/python-client-api-reference.html)
for the up-to-date information.

## Connect to CWM MinIO Instance

The following snippet creates and establishes an HTTPS connection on port 443
with a CWM MinIO instance:

```python
from minio import Minio
from minio.error import S3Error

client = Minio(
    "123345678.eu-test.cloudwm-obj.com",
    access_key="<ACCESS_KEY>",
    secret_key="<SECRET_KEY>",
)
```

For more details, please refer to the official docs:

- [Python Client: Quickstart Guide](https://docs.min.io/docs/python-client-quickstart-guide.html)
- [Python Client: API Reference](https://docs.min.io/docs/python-client-api-reference.html)
