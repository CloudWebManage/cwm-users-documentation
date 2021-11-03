# cwm-users-documentation

## Object Storage

CWM Object Storage is a serverless storage service based on the open source [MinIO](https://min.io/) project.
On every request to your instance endpoint a Minio server is started and used for subsequent requests.
The server is scaled up and down based on usage. Each instance has a dedicated, elastic storage which 
is kept securely regardless of the status of the Minio server, or usage.

The following sections provides the documentation for the end-users of the CWM
Object Storage solution:

- [How To Guides](objectstorage/howto.md)
- [Console](objectstorage/console.md)
- [CLI](objectstorage/cli.md)
- SDKs:
  - [Python](objectstorage/sdk-python.md)
  - [Javascript](objectstorage/sdk-javascript.md)
  - [.NET](objectstorage/sdk-dotnet.md)
