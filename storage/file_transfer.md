# Transferring files to/from Star

Only ssh type of access is open to star. Therefore to upload or
download data only scp and sftp can be used.

To transfer data to and from star use the following address:

    star.uit.no

This address has nodes with 10Gb network interfaces.

Basic tools (scp, sftp)

Standard scp command and sftp clients can be used:

    ssh star.uit.no
    ssh -l <username> star.uit.no

    sftp star.uit.no
    sftp <username>@star.uit.no

## Mounting the file system on you local machine using sshfs

For linux users:

    sshfs [user@]star.uit.no:[dir] mountpoint [options]

eg.:

    sshfs steinar@star.uit.no:  /home/steinar/star-fs/

Windows users may buy and install
[expandrive](https://www.expandrive.com/windows).

### High-performance tools

## NONE cipher

This cipher has the highest transfer rate. Keep in mind that data after
authentication is NOT encrypted, therefore the files can be sniffed and
collected unencrypted by an attacker. To use you add the following to
the client command line:

    -oNoneSwitch=yes -oNoneEnabled=yes

Anytime the None cipher is used a warning will be printed on the screen:

    "WARNING: NONE CIPHER ENABLED"

If you do not see this warning then the NONE cipher is not in use.

MT-AES-CTR

If for some reason (eg: high confidentiality) NONE cipher can't be used,
the multithreaded AES-CTR cipher can be used, add the following to the
client command line (choose one of the numbers):

    -oCipher=aes[128|192|256]-ctr

or:

    -caes[128|192|256]-ctr.

## Subversion and rsync

The tools subversion and rsync is also available for transferring files.
