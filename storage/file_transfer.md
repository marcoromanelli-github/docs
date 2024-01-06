# File Transfer to/from Star

Star supports file transfers primarily through SCP and SFTP, both of which operate over SSH.

## File Transfer Address

Currently, there are no dedicated nodes for file transfers at Star. All transfers should be conducted over the login node using the standard server address.

## Basic Tools (SCP, SFTP)

Standard SCP and SFTP clients can be used for secure file transfers. Here are the basic commands for using these tools:

    ssh star.hofstra.edu
    ssh -l <username> star.hofstra.edu

    sftp star.hofstra.edu
    sftp <username>@star.hofstra.edu

## Mounting the File System on Your Local Machine Using SSHFS

Star HPC Cluster allows users to mount remote file systems on their local machines. For Linux, the command would look like this:

    sshfs [user@]star.hofstra.edu:[dir] mountpoint [options]

For example:

    sshfs yourusername@star.hofstra.edu: /home/yourusername/star-fs/

Windows and Mac users can use Cyberduck for similar functionality. WinSCP is another option for Windows, and FileZilla can be used across Windows, Mac, and Linux.

### High-Performance Tools

For large data transfers, the performance can vary greatly depending on the source's location and bandwidth. Hofstra does not have unlimited Internet bandwidth, so transfers from external sources might be slower. For high-performance transfers, users are encouraged to use utilities like rsync, which is supported and recommended for its efficiency.

## Subversion and Rsync

Rsync is particularly useful and recommended for transferring files to and from the Star HPC Cluster. It provides an efficient way to sync files and directories across different locations while minimizing data transfer.

## Guidelines for Large File Transfers

When transferring very large files or datasets, it is advised to use rsync and to calculate and confirm checksums to ensure data integrity.

## Network Interfaces and Bandwidth

All file transfer access to the Star HPC Cluster is currently through the login node's 1GbE interface. Users should be aware of potential bandwidth limitations, especially when transferring large amounts of data.

## User Authentication and Permissions

File transfers are authenticated in the same way as SSH access. SSH keys are the preferred method for secure authentication, although password authentication is currently allowed. Plans for implementing Multi-Factor Authentication (MFA) are being considered for future security enhancements.
