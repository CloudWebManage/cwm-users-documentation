# Object Storage CLI

- [Overview](#overview)
- [Install](#install)
- [Configure CWM Object Storage Instance](#configure-cwm-object-storage-instance)
- [Example](#example)
- [References](#references)

## Overview

To access CWM Object Storage instance from a CLI, it is recommended to install
and use the official MinIO Client (`mc`), a CWM Object Storage compatible CLI
tool, that provides a modern alternative to UNIX commands such as `ls`, `cat`,
`cp`, `mirror`, `diff`, `find`, etc.

## Install

Please
[install](https://github.com/minio/mc/blob/master/docs/minio-client-complete-guide.md#1--download-minio-client)
the MinIO Client according to your OS and architecture.

## Configure CWM Object Storage Instance

To access your CWM Object Storage active instance, you need to first configure your
instance with the installed MinIO Client (`mc`). You can do so with the `mc`
subcommand `alias` which requires the API Access URL, Access Key, and the Secret
Key. You can find this information under the INFO tab of your instance from the
WebUI.

Syntax:

```shell
mc alias set <ALIAS> <API_ACCESS_URL> <ACCESS_KEY> <SECRET_KEY>
```

Example:

```shell
mc alias set cwm-minio https://123456789.eu.cloudwm-obj.com AAAABBBB CCCCDDDD
```

After setting the alias for a CWM instance e.g. `cwm-minio`, you can
conveniently use it with other `mc` subcommands.

Example:

```shell
mc ls cwm-minio
```

## Example

The following example shows the commands to set an alias, create a bucket,
upload and download an object, remove an object and an empty bucket:

```shell
# set alias
mc alias set cwm-minio https://123456789.eu.cloudwm-obj.com AAAABBBB CCCCDDDD

# create bucket
mc mb cwm-minio/bucket123

# create test object
echo 'test object data' > object.txt

# upload object to bucket
mc cp object.txt cwm-minio/bucket123

# show the contents of the uploaded object
mc cat cwm-minio/bucket123/object.txt

# download object from bucket
mc cp cwm-minio/bucket123/object.txt .

# remove the object
mc rm cwm-minio/bucket123/object.txt

# remove empty bucket
mv rb cwm-minio/bucket123
```

## References

To list all the supported commands, run `mc --help`.

For more details, please refer to
[MinIO Client Complete Guide](https://github.com/minio/mc/blob/master/docs/minio-client-complete-guide.md).
