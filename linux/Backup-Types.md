# Backup Types

https://parablu.com/demystifying-data-backups-types-of-backups/
https://spanning.com/blog/types-of-backup-understanding-full-differential-incremental-backup/


## System Image
- is a copy of the OS binaries, configuration files, and anything else needed to boot the Linux system
- aka Clone

## Full
- copy of all the data, *ignoring* its modification date 
- Needs no other backup types to restore a system fully
- Adv:
	- takes lot less time than others to restore
- DisAdv:
	- takes longer to create

## Incremental
- Only makes a copy of data that has been modified since the last backup operation (any type of backup operation)
- A file's modification timestamp is compared to the last backup's timestamp
- Adv:
	- takes lot less time to create
	- requires a lot less storage space
- DisAdv:
	- data restoration time can be significant
		- because have to start over from a full backup and apply all intermittent incremental backups

## Differential
- makes a copy of all data that has changed since the last full backup
- Considered a good balance b/w full and incremental.
- Adv:
	- takes less time than a full backup but more time than incremental
	- requires less storage than full but more space than incremental
	- Takes a lot less time to restore compared to incremental
		- coz only the full backup and the latest differential backup are needed
- For optimization purposes, requires a full backup to be completed periodically


## Snapshot
- considered a hybrid approach, and is a slightly different flavor of backups.
- Process
	- a full, read-only copy of the data is made to backup media
	- Then pointers (such as hard links) are employed to create a reference table linking the backup data with the original data.
	- The next time a backup is made, an incremental backup is made (instead of a full backup)
		- incremental: only modified or new files are copied to the backup media
		- the pointer reference is copied and updated
	- above step saves space 
		- as only modified files and the updated pointer reference table needs to be stored for each additional backup
- With snapshot, you can go back to any point in time and do a full system restore from that point.
- Also uses a lot less space compared to other backup types
- In essence, simulate multiple full backups per day without taking up the same space or requiring the same processing power as a full backup would.
- `rsync` uses this method