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

You can encrypy using either a standard PGP recipient or using a password.
You'll also need to provide the list of repositories to backup and the name
of the bucket into which the backup will be saved.

**N.B.** That at the moment this only supports repositories in the
`git@git:group/repository.git` format since it stores repositories by user or
project.

### Recipient Encryption

```shell
# List of recipients can be email addresses or key ids.
RECIPIENTS=('you@example.com')

# List of repositories to backup:
REPOS=('git@github.com:mfinelli/git-backup.git'
       'git@github.com:mfinelli/git-backup.wiki.git')

# You can list your buckets with `s3cmd ls`
BUCKET="your-backup-bucket"
```

### Password Encryption

```shell
# Choose a long, strong passphrase to encrypt the repositories with.
PASSPHRASE="SomeReallyGoodPassword"

# List of repositories to backup:
REPOS=('git@github.com:mfinelli/git-backup.git'
       'git@github.com:mfinelli/git-backup.wiki.git')

# You can list your buckets with `s3cmd ls`
BUCKET="your-backup-bucket"
```

## Usage

Running a backup is easy! After configuring everything just run the
`gitbackup` script!

Note that repositories are cloned using the `--mirror` option. This means that
you can restore them to a new remote like so:

```shell
$ git push --mirror "$NEW_REMOTE"
```

### Crontab

You might also find it helpful to run the backup as a cron job. Below is an
example that runs the backup everyday at 0230, but this could obviously adjust
it to suit your needs. I also have my s3 bucket set to delete backups older
than three months. Assuming you have the script deployed to your home
directory:

```
30 2 * * * cd /home/user/git-backup && /home/user/git-backup/gitbackup
```
