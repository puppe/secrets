# Secrets

Secrets is a simple, little program that copies files which contain
secrets like passwords or keys from one UNIX computer (the  *host*) to
another (the *target*) using SSH. The files to be copied as well as
their desired permissions on the target computer are specified in a
simple JSON file.

## Dependencies

* [Racket](https://racket-lang.org/) (on the host)
* [OpenSSH](https://www.openssh.com/)
* the base64 command line tool
* Bash (on the target)
* [sudo](https://www.sudo.ws/) (on the target)

SSH client or server implementations other than OpenSSH may work as
well.

## Usage

```
secrets [<config-file>]
```

Secrets will look for a configuration file named secrets.json in the
current working directory. An alternative configuration file can be
supplied as a command-line argument. Secrets will not work without a
configuration file.

**WARNING**: Secrets copies all files to a single directory. All other
files which are in that directory **will be deleted**.

## Configuration file

Example:

```json
{
    "target": "johndoe@example.org",
    "basedir": "/path/to/dir",
    "files": [
        "path/to/file",
        {
            "source": "path/to/other/file",
            "name": "foobar",
            "owner": "root",
            "group": "users",
            "mode": "640"
        }
    ]
}
```

### Top-level object

The content of your configuration file should be a JSON object with the
following keys:

*target* (required): the target computer which the files are copied to.
This can be anything that is accepted by the OpenSSH client as a
destination. See `man ssh` for details.

*basedir* (optional): the directory on the target which files are copied
to. All other files which are in that directory **will be deleted**. The
default value for this key is `"/var/lib/secrets"`.

*files* (required): a list of files that will be copied to the target.
See the next section for details.

### File specifications

A file can be specified in two ways, either as an object or as a simple
string. If a file is specified as an object, the object should have the
following keys:

*source* (required): the path to a file that will be copied to the
target. If the path is relative, it will be interpreted relatively to
the location of the configuration file.

*name* (optional): the name of the file on the target computer. The
default value for this key is the name of the file on the host computer.

*owner* (optional): the owner of the file on the target computer. The
default value for this key is `"root"`.

*group* (optional): the group of the file on the target computer. The
default value for this key is `"root"`.

*mode* (optional): the mode of the file on the target computer. See `man
chmod` for details. The default value for this key is `"400"`.

If a file is specified as a string "path/to/file", this is equivalent to

```
{
    "source": "path/to/file",
}
```


## Copyright

Copyright 2020 Martin Puppe

This file is part of Secrets.

Secrets is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

Secrets is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with Secrets.  If not, see <https://www.gnu.org/licenses/>.
