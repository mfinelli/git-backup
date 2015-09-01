# Git Backup

Backup git repositories to S3.

## Configuration

Configuration for the gitbackup is saved in your home directory in the
`.gitbackup` file. It is basically just another bash script that defines the
variables that we need which we source before running the backup.

The two variables that you need to set are for the password to encrypt the
repositories with, and a list of repositories to backup:

```shell
# Choose a long, strong passphrase to encrypt the repositories with.
PASSPHRASE="SomeReallyGoodPassword"

# List of repositories to backup:
REPOS=('git@github.com:mfinelli/git-backup.git'
       'git@github.com:mfinelli/git-backup.wiki.git')
```

**N.B.** That at the moment this only supports repositories in the
`git@git:group/repository.git` format since it stores repositories by user or
project.
