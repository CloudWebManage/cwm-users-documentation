# Object Storage CLI

- [Overview](#overview)
- [Install](#install)
  - [macOS](#macos)
  - [GNU/Linux](#gnulinux)
  - [Microsoft Windows](#microsoft-windows)
- [Configure CWM MinIO Instance](#configure-cwm-minio-instance)
- [Global Options](#global-options)
- [Commands](#commands)
  - [Command: `alias`](#command-alias)
  - [Command: `update`](#command-update)
  - [Command: `ls`](#command-ls)
  - [Command: `tree`](#command-tree)
  - [Command: `mb`](#command-mb)
  - [Command: `rb`](#command-rb)
  - [Command: `du`](#command-du)
  - [Command: `cat`](#command-cat)
  - [Command: `sql`](#command-sql)
  - [Command: `head`](#command-head)
  - [Command: `pipe`](#command-pipe)
  - [Command: `cp`](#command-cp)
  - [Command: `mv`](#command-mv)
  - [Command: `rm`](#command-rm)
  - [Command: `share`](#command-share)
    - [Subcommand: `share download`](#subcommand-share-download)
    - [Subcommand: `share upload`](#subcommand-share-upload)
    - [Subcommand: `share list`](#subcommand-share-list)
  - [Command: `mirror`](#command-mirror)
  - [Command: `find`](#command-find)
  - [Command: `diff`](#command-diff)
    - [Option `--json`](#option---json)
    - [Diff values in json output](#diff-values-in-json-output)
  - [Command: `policy`](#command-policy)
  - [Command: `tag`](#command-tag)
  - [Command: `stat`](#command-stat)

## Overview

To access CWM MinIO instance from a CLI, it is recommended to install and use
the official MinIO Client (`mc`) that provides a modern alternative to UNIX
commands such as `ls`, `cat`, `cp`, `mirror`, `diff`, `find`, etc.

## Install

According to your OS and architecture, download and install the relevant MinIO
Client.

### macOS

Install `mc` package using [Homebrew](http://brew.sh/):

```shell
brew install minio/stable/mc
mc --help
```

### GNU/Linux

| Architecture |                         URL                          |
| :----------: | :--------------------------------------------------: |
| 64-bit Intel |  https://dl.min.io/client/mc/release/linux-amd64/mc  |
|  64-bit PPC  | https://dl.min.io/client/mc/release/linux-ppc64le/mc |

```shell
wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc
./mc --help
```

### Microsoft Windows

| Architecture |                           URL                            |
| :----------: | :------------------------------------------------------: |
| 64-bit Intel | https://dl.min.io/client/mc/release/windows-amd64/mc.exe |

```shell
mc.exe --help
```

## Configure CWM MinIO Instance

To access your CWM MinIO active instance, you need to first configure your
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

## Global Options

`mc` supports these optional global flags also:

```shell
GLOBAL FLAGS:
  --autocompletion              install auto-completion for your shell
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help
  --version, -v                 print the version
```

You can set these flags before any subcommand.

Syntax:

```shell
mc [FLAGS] COMMAND ...
```

Example:

```shell
mc --debug ls cwm-minio
```

## Commands

To list all the supported commands, run `mc --help`.

For CWM MinIO instances, the following commands are supported:

```text
alias       set, remove and list aliases in configuration file
ls          list buckets and objects
mb          make a bucket
rb          remove a bucket
cp          copy objects
mirror      synchronize object(s) to a remote site
cat         display object contents
head        display first 'n' lines of an object
pipe        stream STDIN to an object
share       generate URL for temporary access to an object
find        search for objects
sql         run sql queries on objects
stat        show object metadata
mv          move objects
tree        list buckets and objects in a tree format
du          summarize disk usage recursively
retention   set retention for object(s)
legalhold   set legal hold for object(s)
diff        list differences in object name, size, and date between two buckets
rm          remove objects
encrypt    manage bucket encryption config
event       manage object notifications
watch       listen for object notification events
undo        undo PUT/DELETE operations
policy      manage anonymous access to buckets and objects
tag         manage tags for bucket(s) and object(s)
ilm         manage bucket lifecycle
version     manage bucket versioning
update      update mc to latest release
```

### Command: `alias`

`alias` command provides a convenient way to manage aliases entries in your
config file `~/.mc/config.json`. It is also OK to edit the config file manually
using a text editor.

```text
$ mc alias --help
NAME:
  mc alias - set, remove and list aliases in configuration file

USAGE:
  mc alias COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]

COMMANDS:
  set, s      set a new alias to configuration file
  list, ls    list aliases in configuration file
  remove, rm  remove an alias from configuration file
```

Example: Manage Config File

```shell
mc alias set cwm-minio https://123456789.eu.cloudwm-obj.com AAAABBBB CCCCDDDD
```

Remove the alias from the config file:

```shell
mc alias remove cwm-minio
```

List all configured aliases:

```shell
mc alias list
```

### Command: `update`

Check for updates from [https://dl.min.io](https://dl.min.io).

```shell
$ mc update --help
Name:
   mc update - update mc to latest release

USAGE:
   mc update [FLAGS]

FLAGS:
  --json      enable JSON lines formatted output
  --help, -h  show help

EXIT STATUS:
  0 - you are already running the most recent version
  1 - new update was applied successfully
 -1 - error in getting update information

EXAMPLES:
  1. Check and update mc:
     $ mc update
```

Example:

```shell
$ mc update
You are already running the most recent version of ‘mc’.
```

### Command: `ls`

`ls` command lists files, buckets and objects. Use `--incomplete` flag to list
partially copied content.

```shell
$ mc ls --help
NAME:
  mc ls - list buckets and objects

USAGE:
  mc ls [FLAGS] TARGET [TARGET ...]

FLAGS:
  --rewind value                list all object versions no later than specified date
  --versions                    list all versions
  --recursive, -r               list recursively
  --incomplete, -I              list incomplete uploads
  --summarize                   display summary information (number of objects, total size)
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help
```

Example: List all buckets on `cwm-minio`.

```shell
mc ls cwm-minio
```

Example: List contents of `bucket1` on `cwm-minio`.

```shell
mc ls cwm-minio/bucket1
```

### Command: `tree`

`tree` command lists buckets and directories in a tree format. Use `--files`
flag to include files/objects in the listing.

```shell
$ mc tree --help
NAME:
  mc tree - list buckets and objects in a tree format

USAGE:
  mc tree [FLAGS] TARGET [TARGET ...]

FLAGS:
  --files, -f                   includes files in tree
  --depth value, -d value       sets the depth threshold (default: -1)
  --rewind value                display tree no later than specified date
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help
```

Example: List all contents on `cwm-minio/bucket1` in a tree format.

```shell
$ mc tree cwm-minio/bucket1
cwm-minio/bucket1
├─ dir_a
├─ dir_b
│  └─ dir_bb
└─ dir_x
   └─ dir_xx
```

Example: List all objects with the state of 3 days earlier.

```shell
$ mc tree --files --rewind 3d cwm-minio/bucket1
cwm-minio/bucket1/
├─ object1
└─ object2
```

### Command: `mb`

`mb` command creates a new bucket on an object storage. On a filesystem, it
behaves like `mkdir -p` command. Bucket is equivalent of a drive or mount point
in file systems and should not be treated as folders. MinIO does not place any
limits on the number of buckets created per user.

```shell
$ mc mb --help
NAME:
  mc mb - make a bucket

USAGE:
  mc mb [FLAGS] TARGET [TARGET...]

FLAGS:
  --region value                specify bucket region; defaults to 'us-east-1' (default: "us-east-1")
  --ignore-existing, -p         ignore if bucket/directory already exists
  --with-lock, -l               enable object lock
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help
```

Example: Create a new bucket named `bucket1`.

```shell
$ mc mb cwm-minio/bucket1
Bucket created successfully ‘cwm-minio/bucket1’.
```

### Command: `rb`

`rb` command removes a bucket and all its contents on an object storage. On a
filesystem, it behaves like `rmdir` command.

> **NOTE**: When a bucket is removed all bucket configurations associated with the
> bucket will also be removed. All objects and their versions will be removed as
> well. If you need to preserve bucket and its configuration - only empty the
> objects and versions in a bucket use `mc rm` instead.

```shell
$ mc rb --help
NAME:
  mc rb - remove a bucket

USAGE:
  mc rb [FLAGS] TARGET [TARGET...]

FLAGS:
  --force                       force a recursive remove operation on all object versions
  --dangerous                   allow site-wide removal of objects
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help
```

Example: Remove a bucket named `bucket1` on `cwm-minio`.

```shell
$ mc rb cwm-minio/bucket1 --force
Bucket removed successfully ‘cwm-minio/bucket1’.
```

### Command: `du`

`du` command summarizes disk usage recursively.

```shell
$ mc du --help
NAME:
  mc du - summarize disk usage recursively

USAGE:
  mc du [FLAGS] TARGET

FLAGS:
  --depth value, -d value       print the total for a folder prefix only if it is N or fewer levels below the command line argument (default: 0)
  --recursive, -r               recursively print the total for a folder prefix
  --rewind value                include all object versions no later than specified date
  --versions                    include all object versions
  --encrypt-key value           encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help
```

Example: Summarize disk usage of `bucket1` on `cwm-minio` recursively.

```shell
mc du cwm-minio/bucket1
```

### Command: `cat`

`cat` command concatenates contents of a file or object to another. You may also
use it to simply display the contents to `stdout`.

```shell
USAGE:
   mc cat [FLAGS] SOURCE [SOURCE...]
FLAGS:
  --rewind value                   display an earlier object version
  --version-id value, --vid value  display a specific version of an object
  --encrypt-key value              encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --help, -h                       show help
ENVIRONMENT VARIABLES:
   MC_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

Example: Display the contents of `cwm-minio/bucket1/test.txt`.

```shell
mc cat cwm-minio/bucket1/test.txt
```

Example: Display the content of an object 10 days earlier.

```shell
mc cat --rewind "10d" cwm-minio/bucket1/test.txt
```

### Command: `sql`

`sql` run SQL queries on objects.

```shell
$ mc sql --help
NAME:
  mc sql - run sql queries on objects

USAGE:
  mc sql [FLAGS] TARGET [TARGET...]

FLAGS:
  --query value, -e value       sql query expression (default: "select * from s3object")
  --recursive, -r               sql query recursively
  --csv-input value             csv input serialization option
  --json-input value            json input serialization option
  --compression value           input compression type
  --csv-output value            csv output serialization option
  --csv-output-header value     optional csv output header
  --json-output value           json output serialization option
  --encrypt-key value           encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help

ENVIRONMENT VARIABLES:
  MC_ENCRYPT_KEY: list of comma delimited prefix=secret values
```

Example: Select all columns on a set of objects recursively.

```shell
mc sql --recursive --query "select * from S3Object" cwm-minio/bucket1/test.csv
```

Example: Run an aggregation query on an object on MinIO.

```shell
mc sql --query "select count(s.power) from S3Object" cwm-minio/iot-devices/power-ratio.csv
```

Example: Run an aggregation query on an encrypted object with customer provided keys.

```shell
mc sql --encrypt-key "cwm-minio/iot-devices=32byteslongsecretkeymustbegiven1" \
    --query "select count(s.power) from S3Object" cwm-minio/iot-devices/power-ratio-encrypted.csv
```

### Command: `head`

`head` display first `n` lines of an object.

```shell
$ mc head --help
NAME:
  mc head - display first 'n' lines of an object

USAGE:
  mc head [FLAGS] SOURCE [SOURCE...]

FLAGS:
  -n value, --lines value          print the first 'n' lines (default: 10)
  --rewind value                   select an object version at specified time
  --version-id value, --vid value  select an object version to display
  --encrypt-key value              encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value     path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                      disable progress bar display
  --no-color                       disable color theme
  --json                           enable JSON lines formatted output
  --debug                          enable debug output
  --insecure                       disable SSL certificate verification
  --help, -h                       show help

ENVIRONMENT VARIABLES:
  MC_ENCRYPT_KEY:  list of comma delimited prefix=secret values

NOTE:
  'mc head' automatically decompresses 'gzip', 'bzip2' compressed objects.
```

Example: Display the first line of a text file `test.txt`.

```shell
mc head -n 1 cwm-minio/bucket1/test.txt
```

Example: Display the first line of a server encrypted object `encryptedobject.txt`.

```shell
mc head -n 1 --encrypt-key "cwm-minio/bucket1=<SECRET_KEY>" cwm-minio/bucket1/encryptedobject.txt
```

Example: Display the first line of the content of an object, 1 year earlier.

```shell
mc head -n 1 --rewind 365d cwm-minio/bucket1/test.txt
```

### Command: `pipe`

`pipe` command copies contents of `stdin` to a target. When no target is
specified, it writes to `stdout`.

```shell
$ mc pipe --help
NAME:
  mc pipe - stream STDIN to an object

USAGE:
  mc pipe [FLAGS] [TARGET]

FLAGS:
  --encrypt value                    encrypt objects (using server-side encryption with server managed keys)
  --storage-class value, --sc value  set storage class for new object(s) on target
  --attr value                       add custom metadata for the object
  --tags value                       apply tags to the uploaded objects
  --encrypt-key value                encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value       path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                        disable progress bar display
  --no-color                         disable color theme
  --json                             enable JSON lines formatted output
  --debug                            enable debug output
  --insecure                         disable SSL certificate verification
  --help, -h                         show help

ENVIRONMENT VARIABLES:
  MC_ENCRYPT:      list of comma delimited prefix values
  MC_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

Example: Stream MySQL database dump directly.

```shell
mysqldump -u root -p ******* accountsdb | mc pipe cwm-minio/backups/accountsdb-oct-9-2015.sql
```

### Command: `cp`

`cp` command copies data from one or more sources to a target. All copy
operations to object storage are verified with MD5Sum checksums. Interrupted or
failed copy operations can be resumed from the point of failure.

```shell
$ mc cp --help
NAME:
  mc cp - copy objects

USAGE:
  mc cp [FLAGS] SOURCE [SOURCE...] TARGET

FLAGS:
  --rewind value                     roll back object(s) to current version at specified time
  --version-id value, --vid value    select an object version to copy
  --recursive, -r                    copy recursively
  --older-than value                 copy objects older than L days, M hours and N minutes
  --newer-than value                 copy objects newer than L days, M hours and N minutes
  --storage-class value, --sc value  set storage class for new object(s) on target
  --encrypt value                    encrypt/decrypt objects (using server-side encryption with server managed keys)
  --attr value                       add custom metadata for the object
  --continue, -c                     create or resume copy session
  --preserve, -a                     preserve filesystem attributes (mode, ownership, timestamps)
  --disable-multipart                disable multipart upload feature
  --md5                              force all upload(s) to calculate md5sum checksum
  --tags value                       apply tags to the uploaded objects
  --retention-mode value             retention mode to be applied on the object (governance, compliance)
  --retention-duration value         retention duration for the object in d days or y years
  --legal-hold value                 apply legal hold to the copied object (on, off)
  --encrypt-key value                encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value       path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                        disable progress bar display
  --no-color                         disable color theme
  --json                             enable JSON lines formatted output
  --debug                            enable debug output
  --insecure                         disable SSL certificate verification
  --help, -h                         show help

ENVIRONMENT VARIABLES:
  MC_ENCRYPT:      list of comma delimited prefixes
  MC_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

Example: Copy a text file to `cwm-minio/bucket1`.

```shell
mc cp test.txt cwm-minio/bucket1
```

Example: Copy a text file to `cwm-minio/bucket1` with specified metadata.

```shell
mc cp --attr key1=value1;key2=value2 test.txt cwm-minio/bucket1
```

Example: Copy a folder recursively from `cwm-minio/bucket1` to Amazon S3 cloud
storage with specified metadata.

```shell
mc cp --attr Cache-Control=max-age=90000,min-fresh=9000\;key1=value1\;key2=value2 --recursive cwm-minio/bucket1/ s3/bucket1/
```

Example: Copy a text file to `cwm-minio/bucket1` and assign storage-class
`REDUCED_REDUNDANCY` to the uploaded object.

```shell
mc cp --storage-class REDUCED_REDUNDANCY test.txt cwm-minio/bucket1
```

Example: Copy a javascript file to object storage and assign `Cache-Control`
header to the uploaded object.

```sh
mc cp --attr Cache-Control=no-cache test.js cwm-minio/bucket1
```

Example: Copy a text file and preserve the filesystem attributes.

```shell
mc cp -a test.txt cwm-minio/bucket1
```

Example: Roll back to object version to 10 days earlier while copying.

```shell
mc cp --rewind 10d cwm-minio/bucket1/test.txt test.txt
```

### Command: `mv`

`mv` command moves data from one or more sources to a target. All move
operations to object storage are verified with MD5Sum checksums. Interrupted or
failed move operations can be resumed from the point of failure.

```shell
$ mc mv --help
NAME:
  mc mv - move objects

USAGE:
  mc mv [FLAGS] SOURCE [SOURCE...] TARGET

FLAGS:
  --recursive, -r                    move recursively
  --older-than value                 move objects older than L days, M hours and N minutes
  --newer-than value                 move objects newer than L days, M hours and N minutes
  --storage-class value, --sc value  set storage class for new object(s) on target
  --encrypt value                    encrypt/decrypt objects (using server-side encryption with server managed keys)
  --attr value                       add custom metadata for the object
  --continue, -c                     create or resume move session
  --preserve, -a                     preserve filesystem attributes (mode, ownership, timestamps)
  --disable-multipart                disable multipart upload feature
  --encrypt-key value                encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value       path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                        disable progress bar display
  --no-color                         disable color theme
  --json                             enable JSON lines formatted output
  --debug                            enable debug output
  --insecure                         disable SSL certificate verification
  --help, -h                         show help

ENVIRONMENT VARIABLES:
  MC_ENCRYPT:      list of comma delimited prefixes
  MC_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

Example: Move a text file to an object storage.

```shell
mc mv test.txt cwm-minio/bucket1
```

Example: Move a text file to an object storage with specified metadata.

```shell
mc mv --attr key1=value1;key2=value2 test.txt cwm-minio/bucket1
```

Example: Move a folder recursively from MinIO to Amazon S3 cloud storage with
specified metadata.

```shell
mc mv --attr Cache-Control=max-age=90000,min-fresh=9000\;key1=value1\;key2=value2 --recursive cwm-minio/bucket1/folder1 s3/bucket1/
```

Example: Move a text file to an object storage and assign storage-class
`REDUCED_REDUNDANCY` to the uploaded object.

```shell
mc mv --storage-class REDUCED_REDUNDANCY test.txt cwm-minio/bucket1
```

Example: Move a server-side encrypted file to an object storage.

```shell
mc mv --recursive --encrypt-key "s3/documents/=<SECRET_KEY_1>, cwm-minio/documents/=<SECRET_KEY_2>" s3/documents/test.txt cwm-minio/documents/
```

### Command: `rm`

Use `rm` command to remove file or object.

```shell
$ mc rm --help
NAME:
  mc rm - remove objects

USAGE:
  mc rm [FLAGS] TARGET [TARGET ...]

FLAGS:
  --versions                       remove object(s) and all its versions
  --non-current                    remove object(s) versions that are non current (with top-level delete marker)
  --rewind value                   roll back object(s) to current version at specified time
  --version-id value, --vid value  delete a specific version of an object
  --recursive, -r                  remove recursively
  --force                          allow a recursive remove operation
  --dangerous                      allow site-wide removal of objects
  --incomplete, -I                 remove incomplete uploads
  --fake                           perform a fake remove operation
  --stdin                          read object names from STDIN
  --older-than value               remove objects older than L days, M hours and N minutes
  --newer-than value               remove objects newer than L days, M hours and N minutes
  --bypass                         bypass governance
  --encrypt-key value              encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value     path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                      disable progress bar display
  --no-color                       disable color theme
  --json                           enable JSON lines formatted output
  --debug                          enable debug output
  --insecure                       disable SSL certificate verification
  --help, -h                       show help

ENVIRONMENT VARIABLES:
  MC_ENCRYPT_KEY: list of comma delimited prefix=secret values
```

Example: Remove a single object.

```shell
mc rm cwm-minio/bucket1/tet.txt
```

Example: Remove an encrypted object.

```shell
mc rm --encrypt-key "cwm-minio/bucket1=<SECRET_KEY>" cwm-minio/bucket1/test.txt
```

Example: Recursively remove a bucket's contents. Since this is a dangerous
operation, you must explicitly pass `--force` option.

```shell
mc rm --recursive --force cwm-minio/bucket1
```

Example: Remove all uploaded incomplete files for an object.

```shell
mc rm --incomplete cwm-minio/bucket1/test.txt
```

Example: Remove object and output a message only if the object is created older
than 1 day, 2 hours and 30 minutes. Otherwise, the command stays quiet and
nothing is printed out.

```shell
mc rm -r --force --older-than 1d2h30m cwm-minio/bucket1
```

### Command: `share`

`share` command securely grants upload or download access to object storage.
This access is only temporary and it is safe to share with remote users and
applications. If you want to grant permanent access, you may look at `mc policy`
command instead.

Generated URL has access credentials encoded in it. Any attempt to tamper the
URL will invalidate the access. To understand how this mechanism works, please
follow [Pre-Signed URL](http://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html)
technique.

```shell
$ mc share --help
NAME:
  mc share - generate URL for temporary access to an object

USAGE:
  mc share COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]

COMMANDS:
  download  generate URLs for download access
  upload    generate `curl` command to upload objects without requiring access/secret keys
  list      list previously shared objects

FLAGS:
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help
```

#### Subcommand: `share download`

`share download` command generates URLs to download objects without requiring
access and secret keys. Expiry option sets the maximum validity period (no more
than 7 days), beyond which the access is revoked automatically.

```shell
$ mc share download --help
NAME:
  mc share download - generate URLs for download access

USAGE:
  mc share download [FLAGS] TARGET [TARGET...]

FLAGS:
  --recursive, -r                  share all objects recursively
  --version-id value, --vid value  share a particular object version
  --expire value, -E value         set expiry in NN[h|m|s] (default: "168h")
  --config-dir value, -C value     path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                      disable progress bar display
  --no-color                       disable color theme
  --json                           enable JSON lines formatted output
  --debug                          enable debug output
  --insecure                       disable SSL certificate verification
  --help, -h                       show help
```

Example: Grant temporary access to an object with 4 hours expiry limit.

```shell
mc share download --expire 4h cwm-minio/bucket1/test.txt
```

Example: Share a particular version of an object.

```shell
mc share download --version-id <VERSION_ID> cwm-minio/bucket1/test.txt
```

#### Subcommand: `share upload`

`share upload` command generates a `curl` command to upload objects without
requiring access/secret keys. Expiry option sets the maximum validity period (no
more than 7 days), beyond which the access is revoked automatically.
`Content-Type` option restricts uploads to only certain type of files.

```shell
$ mc share upload --help
NAME:
  mc share upload - generate `curl` command to upload objects without requiring access/secret keys

USAGE:
  mc share upload [FLAGS] TARGET [TARGET...]

FLAGS:
  --recursive, -r                 recursively upload any object matching the prefix
  --expire value, -E value        set expiry in NN[h|m|s] (default: "168h")
  --content-type value, -T value  specify a content-type to allow
  --config-dir value, -C value    path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                     disable progress bar display
  --no-color                      disable color theme
  --json                          enable JSON lines formatted output
  --debug                         enable debug output
  --insecure                      disable SSL certificate verification
  --help, -h                      show help
```

Example: Generate a `curl` command to enable upload access to
`cwm-minio/bucket1/test.txt`. User replaces `<FILE>` with the actual filename to
upload.

```shell
mc share upload cwm-minio/bucket1/test.txt
```

#### Subcommand: `share list`

`share list` command lists unexpired URLs that were previously shared.

```shell
$ mc share list --help
NAME:
  mc share list COMMAND - list previously shared objects

USAGE:
  mc share list COMMAND

COMMAND:
  upload:   list previously shared access to uploads.
  download: list previously shared access to downloads.
```

### Command: `mirror`

`mirror` command synchronizes data between filesystems and object storages,
similarly to `rsync`.

```shell
mc mirror --help
NAME:
  mc mirror - synchronize object(s) to a remote site

USAGE:
  mc mirror [FLAGS] SOURCE TARGET

FLAGS:
  --overwrite                        overwrite object(s) on target if it differs from source
  --fake                             perform a fake mirror operation
  --watch, -w                        watch and synchronize changes
  --remove                           remove extraneous object(s) on target
  --region value                     specify region when creating new bucket(s) on target (default: "us-east-1")
  --preserve, -a                     preserve file(s)/object(s) attributes and bucket(s) policy/locking configuration(s) on target bucket(s)
  --md5                              force all upload(s) to calculate md5sum checksum
  --active-active                    enable active-active multi-site setup
  --disable-multipart                disable multipart upload feature
  --exclude value                    exclude object(s) that match specified object name pattern
  --older-than value                 filter object(s) older than L days, M hours and N minutes
  --newer-than value                 filter object(s) newer than L days, M hours and N minutes
  --storage-class value, --sc value  specify storage class for new object(s) on target
  --encrypt value                    encrypt/decrypt objects (using server-side encryption with server managed keys)
  --attr value                       add custom metadata for all objects
  --monitoring-address value         if specified, a new prometheus endpoint will be created to report mirroring activity. (eg: localhost:8081)
  --encrypt-key value                encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value       path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                        disable progress bar display
  --no-color                         disable color theme
  --json                             enable JSON lines formatted output
  --debug                            enable debug output
  --insecure                         disable SSL certificate verification
  --help, -h                         show help

ENVIRONMENT VARIABLES:
   MC_ENCRYPT:      list of comma delimited prefixes
   MC_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

Example: Mirror a local directory to `cwm-minio/bucket1`.

```shell
mc mirror ./data/ cwm-minio/bucket1
```

Example: Continuously watch for changes on a local directory and mirror the
changes to `cwm-minio/bucket1`.

```shell
mc mirror -w ./data/ cwm-minio/bucket1
```

### Command: `find`

`find` command finds files which match the given set of parameters. It only
lists the contents which match the given set of criteria.

```shell
mc find --help
NAME:
  mc find - search for objects

USAGE:
  mc find PATH [FLAGS]

FLAGS:
  --exec value                  spawn an external process for each matching object (see FORMAT)
  --ignore value                exclude objects matching the wildcard pattern
  --name value                  find object names matching wildcard pattern
  --newer-than value            match all objects newer than L days, M hours and N minutes
  --older-than value            match all objects older than L days, M hours and N minutes
  --path value                  match directory names matching wildcard pattern
  --print value                 print in custom format to STDOUT (see FORMAT)
  --regex value                 match directory and object name with PCRE regex pattern
  --larger value                match all objects larger than specified size in units (see UNITS)
  --smaller value               match all objects smaller than specified size in units (see UNITS)
  --maxdepth value              limit directory navigation to specified depth (default: 0)
  --watch                       monitor a specified path for newly created object(s)
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help

UNITS
  --smaller, --larger flags accept human-readable case-insensitive number
  suffixes such as "k", "m", "g" and "t" referring to the metric units KB,
  MB, GB and TB respectively. Adding an "i" to these prefixes, uses the IEC
  units, so that "gi" refers to "gibibyte" or "GiB". A "b" at the end is
  also accepted. Without suffixes the unit is bytes.

  --older-than, --newer-than flags accept the string for days, hours and minutes
  i.e. 1d2h30m states 1 day, 2 hours and 30 minutes.

FORMAT
  Support string substitutions with special interpretations for following keywords.
  Keywords supported if target is filesystem or object storage:

     {}     --> Substitutes to full path.
     {base} --> Substitutes to basename of path.
     {dir}  --> Substitutes to dirname of the path.
     {size} --> Substitutes to object size of the path.
     {time} --> Substitutes to object modified time of the path.

  Keywords supported if target is object storage:

     {url} --> Substitutes to a shareable URL of the path.
```

Example: Find all JPEG images from S3 bucket and copy to MinIO "play/bucket"
bucket continuously.

```shell
mc find s3/bucket --name "*.jpg" --watch --exec "mc cp {} cwm-minio/bucket1"
```

### Command: `diff`

``diff`` command computes the differences between the two directories. It only
lists the contents which are missing or which differ in size.

It *DOES NOT* compare the contents, so it is possible that the objects which are
of same name and of the same size, but have difference in contents are not
detected. This way, it can perform high speed comparison on large volumes or
between sites

```shell
$ mc diff --help
NAME:
  mc diff - list differences in object name, size, and date between two buckets

USAGE:
  mc diff [FLAGS] FIRST SECOND

FLAGS:
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help

DESCRIPTION:
  Diff only calculates differences in object name, size and time. It *DOES NOT* compare objects' contents.

LEGEND:
  < - object is only in source.
  > - object is only in destination.
  ! - newer object is in source.
```

Example: Compare a local directory and a remote object storage.

```shell
mc diff ./data cwm-minio/bucket1
```

#### Option `--json`

JSON option enables parsable output in [JSON lines](http://jsonlines.org/)
format.

Example: `diff` JSON output.

```shell
mc diff cmw-minio/bucket1 cwm-minio/bucket2 --json
```

#### Diff values in json output

|       Constant        | Value |                 Meaning                 |
| :-------------------: | :---: | :-------------------------------------: |
|    differInUnknown    |   0   |   Could not perform diff due to error   |
|     differInNone      |   1   |             Does not differ             |
|     differInSize      |   2   |             Differs in size             |
|   differInMetadata    |   3   |           Differs in metadata           |
|     differInType      |   4   |    Differs in type exfile/directory     |
|     differInFirst     |   5   |         Only in source (FIRST)          |
|    differInSecond     |   6   |         Only in target (SECOND)         |
| differInAASourceMTime |   7   | Differs in active-active source modtime |

### Command: `policy`

Manage anonymous bucket policies to a bucket and its contents.

```shell
$ mc policy --help
Name:
  mc policy - manage anonymous access to buckets and objects

USAGE:
  mc policy [FLAGS] set PERMISSION TARGET
  mc policy [FLAGS] set-json FILE TARGET
  mc policy [FLAGS] get TARGET
  mc policy [FLAGS] get-json TARGET
  mc policy [FLAGS] list TARGET

FLAGS:
  --recursive, -r               list recursively
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help

PERMISSION:
  Allowed policies are: [none, download, upload, public].

FILE:
  A valid S3 policy JSON filepath.
```

Example: Show current anonymous bucket policy.

Show current anonymous bucket policy for `bucket1/data/2020/` subdirectory.

```shell
mc policy get cwm-minio/bucket1/data/2020/
```

Example: Set anonymous bucket policy to download only.

Set anonymous bucket policy for `bucket1/data/2020/` sub-directory and its
objects to `download` only. The objects under the subdirectory will be publicly
accessible.

```shell
mc policy set download cwm-minio/bucket1/data/2020/
```

Example: Set anonymous bucket policy from a JSON file.

Configure bucket policy for `bucket1` with a policy JSON file.

```shell
mc policy set-json ./policy.json cwm-minio/bucket1
```

Example: Set current anonymous bucket policy to private.

Set anonymous bucket policy for `bucket1/data/2020/` subdirectory to
`private`. This is equivalent to removing any bucket policies.

```shell
mc policy set private cwm-minio/bucket1/data/2020/
```

### Command: `tag`

`tag` command provides a convenient way to set, remove, and list bucket/object
tags. Tags are defined as key-value pairs.

```shell
$ mc tag --help
NAME:
  mc tag - manage tags for bucket and object(s)

USAGE:
  mc tag COMMAND [COMMAND FLAGS | -h] [ARGUMENTS...]

COMMANDS:
  list    list tags of a bucket or an object
  remove  remove tags assigned to a bucket or an object
  set     set tags for a bucket and object(s)

FLAGS:
  --config-dir value, -C value  path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                   disable progress bar display
  --no-color                    disable color theme
  --json                        enable JSON lines formatted output
  --debug                       enable debug output
  --insecure                    disable SSL certificate verification
  --help, -h                    show help
```

Example: List tags assigned to an object.

```shell
mc tag list cwm-minio/bucket1/test.txt
```

Example: Set tags for an object.

```shell
mc tag set cwm-minio/bucket1/test.txt "key1=value1&key2=value2&key3=value3"
```

Example: Remove tags assigned to an object.

```shell
mc tag remove cwm-minio/bucket1/test.txt
```

Example: Assign tags to a object versions older than one week.

```shell
mc tag set --versions --rewind 7d cwm-minio/bucket1/test.txt "status=old"
```

### Command: `stat`

`stat` command displays information on objects (with optional prefix) contained
in the specified bucket on an object storage. On a filesystem, it behaves like
`stat` command.

```shell
$ mc stat --help
NAME:
  mc stat - show object metadata

USAGE:
  mc stat [FLAGS] TARGET [TARGET ...]

FLAGS:
  --rewind value                   stat on older version(s)
  --versions                       stat all versions
  --version-id value, --vid value  stat a specific object version
  --recursive, -r                  stat all objects recursively
  --encrypt-key value              encrypt/decrypt objects (using server-side encryption with customer provided keys)
  --config-dir value, -C value     path to configuration folder (default: "/home/cwm/.mc")
  --quiet, -q                      disable progress bar display
  --no-color                       disable color theme
  --json                           enable JSON lines formatted output
  --debug                          enable debug output
  --insecure                       disable SSL certificate verification
  --help, -h                       show help

ENVIRONMENT VARIABLES:
  MC_ENCRYPT_KEY:  list of comma delimited prefix=secret values
```

Example: Display information on a bucket.

```shell
mc stat cwm-minio/bucket1
```

Example: Display information on an encrypted object.

```shell
mc stat cwm-minio/bucket1/test.txt --encrypt-key "cwm-minio/bucket1=<SECRET_KEY>"
```

Example: Display information on objects contained in the bucket.

```shell
mc stat -r cwm-minio/bucket1
```

Example: Stat a specific object version.

```shell
mc stat --version-id "<VERSION_ID>" cwm-minio/bucket1/test.txt
```
