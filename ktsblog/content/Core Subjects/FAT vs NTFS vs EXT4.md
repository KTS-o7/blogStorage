+++
title = 'FAT vs NTFS vs EXT4'
date = 2024-08-29T18:59:52+05:30
draft = false
series = 'Operating System'
tags =['file-system','os']
toc = false
+++

## Filesystems: FAT vs. NTFS vs. ext4

### FAT

FAT (File Allocation Table) is an older file system that is widely compatible with various devices and legacy systems.

### NTFS

NTFS (New Technology File System) is a modern file system used primarily with Windows, offering advanced features and better performance.

### ext4

ext4 (Fourth Extended Filesystem) is a widely used file system in Linux environments, known for its robustness and performance.

### Key Differences

| Feature                           | FAT                                                           | NTFS                                                                            | ext4                                                                          |
| --------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **File Size and Volume Limits**   | Supports files up to 4GB and volumes up to 2TB                | Handles files up to 16TB and volumes up to 256TB                                | Supports files up to 16TB and volumes up to 1EB                               |
| **File Permissions and Security** | No built-in file permissions or encryption                    | Supports ACLs, encryption (EFS), and advanced security features                 | Supports Unix-style permissions and ACLs                                      |
| **Journaling**                    | No journaling; file system integrity must be checked manually | Uses journaling for reliability and recovery                                    | Supports journaling for improved reliability and quick recovery               |
| **Compression and Quotas**        | No support for file compression or disk quotas                | Supports file/folder compression and disk quotas                                | No native support for file compression, but supports disk quotas              |
| **Compatibility**                 | Highly compatible with various devices and legacy systems     | Primarily used with Windows; limited compatibility with other operating systems | Primarily used with Linux; limited compatibility with other operating systems |
| **Performance**                   | Faster for simple tasks but less efficient with large files   | Better performance with large files and volumes                                 | Generally offers good performance, especially with large files and volumes    |
