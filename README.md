# Git Backup

Backup git repositories to S3.

## Dependencies

### cfv

Checksums (SHA1) of every file are generated before tar'ing and encrypting
the repositories using `cfv`. You should install it using your favorite
package manager before trying to run a backup.

### s3cmd

[s3cmd](https://github.com/s3tools/s3cmd) is used to actuall upload the
backups to AWS, so you will need to install and configure it first. Just like
above, install with your package manager and then configure the tool with:

```shell
$ s3cmd --configure
```

## Configuration

Configuration for the gitbackup is saved in your home directory in the
`.gitbackup` file. It is basically just another bash script that defines the
variables that we need which we source before running the backup.

The three variables that you need to set are for the password to encrypt the
repositories with, a list of repositories to backup, and the name of the
bucket into which the backup will be saved:

```shell
# Choose a long, strong passphrase to encrypt the repositories with.
PASSPHRASE="SomeReallyGoodPassword"

# List of repositories to backup:
REPOS=('git@github.com:mfinelli/git-backup.git'
       'git@github.com:mfinelli/git-backup.wiki.git')

# You can list your buckets with `s3cmd ls`
BUCKET="your-backup-bucket"
```

**N.B.** That at the moment this only supports repositories in the
`git@git:group/repository.git` format since it stores repositories by user or
project.
