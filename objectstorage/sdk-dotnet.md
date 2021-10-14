# Object Storage .NET SDK

## Overview

.NET can also be used to access the CWM MinIO instances. The .NET
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
[API reference](https://docs.min.io/docs/javascript-client-api-reference.html)
for the up-to-date information.

## Connect to CWM MinIO Instance

The following snippet creates and establishes an HTTPS connection on port 443
with a CWM MinIO instance:

```csharp
using Minio;

private static MinioClient minio = new MinioClient("12345678.eu-test.cloudwm-obj.com",
                "<ACCESS_KEY>",
                "<SECRET_KEY>"
                ).WithSSL();
```

For more details, please refer to the official docs:

- [.NET Client: QuickStart Guide](https://docs.min.io/docs/dotnet-client-quickstart-guide.html)
- [.NET Client: API Reference](https://docs.min.io/docs/dotnet-client-api-reference.html)
