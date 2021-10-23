# Object Storage JavaScript SDK

## Overview

JavaScript can also be used to access the CWM Object Storage instances. The
JavaScript SDK provides all the APIs to perform operations similar to MinIO
Client (`mc`).

For more details on the MinIO Client, please see the [CLI](cli.md)
documentation.

## Default Ports

The default ports for a CWM Object Storage instance are:

- HTTP: 80
- HTTPS: 443

## Endpoint Prefix

The endpoint listed for a CWM Object Storage instance i.e. **ACCESS API URL** on
the CWM Object Storage Management Console may contain the protocol prefix e.g.
`https://`. You do not need the prefix with the JavaScript API to connect to the
CWM Object Storage instance.

You can consult the
[API reference](https://docs.min.io/docs/javascript-client-api-reference.html)
for the up-to-date information.

## Connect to CWM Object Storage Instance

The following snippet creates and establishes an HTTPS connection on port 443
with a CWM Object Storage instance:

```javascript
var Minio = require('minio');

var minioClient = new Minio.Client({
    endPoint: '12345678.eu-test.cloudwm-obj.com',
    useSSL: true,
    accessKey: '<ACCESS_KEY>',
    secretKey: '<SECRET_KEY>'
});
```

For more details, please refer to the official docs:

- [JavaScript Client: QuickStart Guide](https://docs.min.io/docs/javascript-client-quickstart-guide.html)
- [JavaScript Client: API Reference](https://docs.min.io/docs/javascript-client-api-reference.html)
