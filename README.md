# backup
A simple, but cross-platform and efficient backup program

This module implements a backup procedure, with optimisations
for NTFS drives.
Source can be any directory, for example C:/
Target is a directory, with each backup getting a subdirectory within it.
For example, C:/snapshots/20101103.
The target is essentially a copy of the source, except that:
  - some files and/or directories may have been excluded, and
  - files and/or directories that have not changed since the previous
    backup will be links (hard for file, sym for dirs to the copies
    in the previous backup dir).
Some state files are maintained in the base target dir:
  - journal for the state of the NTFS journal from the last
  backup, including a dir map.
  - previous for the previous successful backup name
  - exclusions is a list of files and dirs to exclude from backups.
Basic algorithm:
  - Input is source directory, target directory, and name.
  - Read exclusions file and journal file.
  - Open journal and build list of changed paths.
  - Iterate over source, using changed paths to optimise if possible.
     - For each file/dir, either copy in or make link to copy in previous
       backup.
  - Save journal file.
  
Example:
backup.py C:/ C:/snapshots 20101103
