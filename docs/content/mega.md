---
title: "Mega"
description: "Rclone docs for Mega"
date: "2018-04-09"
---

<i class="fa fa-archive"></i> Mega
-----------------------------------------

[Mega](https://mega.nz/) is a cloud storage and file hosting service
known for its security feature where all files are encrypted locally
before they are uploaded. This prevents anyone (including employees of
Mega) from accessing the files without knowledge of the key used for
encryption.

This is an rclone backend for Mega which supports the file transfer
features of Mega using the same client side encryption.

Paths are specified as `remote:path`

Paths may be as deep as required, eg `remote:directory/subdirectory`.

Here is an example of how to make a remote called `remote`.  First run:

     rclone config

This will guide you through an interactive setup process:

```
No remotes found - make a new one
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> remote
Type of storage to configure.
Choose a number from below, or type in your own value
 1 / Alias for a existing remote
   \ "alias"
[snip]
14 / Mega
   \ "mega"
[snip]
23 / http Connection
   \ "http"
Storage> mega
User name
user> you@example.com
Password.
y) Yes type in my own password
g) Generate random password
n) No leave this optional password blank
y/g/n> y
Enter the password:
password:
Confirm the password:
password:
Remote config
--------------------
[remote]
type = mega
user = you@example.com
pass = *** ENCRYPTED ***
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
```

**NOTE:** The encryption keys need to have been already generated after a regular login
via the browser, otherwise attempting to use the credentials in `rclone` will fail.

Once configured you can then use `rclone` like this,

List directories in top level of your Mega

    rclone lsd remote:

List all the files in your Mega

    rclone ls remote:

To copy a local directory to an Mega directory called backup

    rclone copy /home/source remote:backup

### Modified time and hashes ###

Mega does not support modification times or hashes yet.

### Duplicated files ###

Mega can have two files with exactly the same name and path (unlike a
normal file system).

Duplicated files cause problems with the syncing and you will see
messages in the log about duplicates.

Use `rclone dedupe` to fix duplicated files.

<!--- autogenerated options start - DO NOT EDIT, instead edit fs.RegInfo in backend/mega/mega.go then run make backenddocs -->
### Standard Options

Here are the standard options specific to mega (Mega).

#### --mega-user

User name

- Config:      user
- Env Var:     RCLONE_MEGA_USER
- Type:        string
- Default:     ""

#### --mega-pass

Password.

- Config:      pass
- Env Var:     RCLONE_MEGA_PASS
- Type:        string
- Default:     ""

### Advanced Options

Here are the advanced options specific to mega (Mega).

#### --mega-debug

Output more debug from Mega.

If this flag is set (along with -vv) it will print further debugging
information from the mega backend.

- Config:      debug
- Env Var:     RCLONE_MEGA_DEBUG
- Type:        bool
- Default:     false

#### --mega-hard-delete

Delete files permanently rather than putting them into the trash.

Normally the mega backend will put all deletions into the trash rather
than permanently deleting them.  If you specify this then rclone will
permanently delete objects instead.

- Config:      hard_delete
- Env Var:     RCLONE_MEGA_HARD_DELETE
- Type:        bool
- Default:     false

<!--- autogenerated options stop -->

### Limitations ###

This backend uses the [go-mega go library](https://github.com/t3rm1n4l/go-mega) which is an opensource
go library implementing the Mega API. There doesn't appear to be any
documentation for the mega protocol beyond the [mega C++ SDK](https://github.com/meganz/sdk) source code
so there are likely quite a few errors still remaining in this library.

Mega allows duplicate files which may confuse rclone.
