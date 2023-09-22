# Commands

- `type -a <proposed_alias_name>`
	- to display any commands, aliases or functions currently using the proposed name
 

- `localectl list-locales`
- `localectl status`

- `# hwclock`
- `# hwclock -r`
- `# hwclock --show`


- `timedatectl list-timezones`
- `timedatectl set-timezone "<time zone>"`
	- changes the timezone
	- Behind the scenes, modifies the sym link b/w `/etc/localtime` and `/usr/share/zoneinfo/<Timezone>`
- `timedatectl set-time <HH:MM:SS | YYYY-MM-DD>`

- `ps -f`
	- lists processes with parent pids and user IDs




- `set`
- `set | grep <variable>`
	- find out if a shell variable has been defined
- `set -o`
	- view status of shell parameters
- `set -o <parameter>`
	- turns on the parameter, e.g. `allexport` which auto-exports all shell variables
- `set +o <parameter>`
	- turn off the parameter


- `env | grep <env_variable>`
	- find out if the env variable has been defined
 

- `<command> 2>&1`
	- redirects std error to the same location as std output 
	- e.g: `ls -ld /etc /doesNotExist > errorfile 2>&1` (redirect both to `errorfile`)
- `<command> 1>&2`
	- redirects std output to the same location as std error
	- e.g.: `ls -ld /etc /doesNotExist 2> errorfile 1>&2`
- `<command> >&`
	- redirects std out and stderr to the same file
	- e.g: `ls -ld /etc /doesNotExist &> errorfile`


- `chage -l <username>`
	- displays password aging information for a user
- `chage <username>`
	- open a text based interface to change user passwd aging info


- `passwd -S <username>`
	- displays password aging information for a user
- `passwd -e <username>`
	- immediately expire a user's password, forcing them to chance password on next login
 - `# passwd -l <username>`
	 - lock a username's password, prevents them from logging in
	 - requires root
 - `# passwd -u <username>`
	 - unlock the username's password as root


- `useradd -r <username>`
	- `-r` indicates that this is a system user
- `useradd -n`
	- negates the setting in `/etc/login.defs` and force the defaults in `/etc/default/useradd` to be used


- `userdel <username>`
	- removes user's record from `/etc/shadow` & `/etc/passwd`, but leaves the home directory
- `userdel -r <username>`
	- removes above + user directory + cron jobs + at jobs and mail


- `usermod -G <secondary_group_name> <username>`
	- add username to secondary group
	- NOTE: overwrites all previous secondary groups
	- use `usermod -aG` option to append
- `usermod -g <group_id> <username>`
	- make group_id for the user
- `usermod -U <username>`
	- unlock the username's account (similar to `passwd -l <username>`)
- `usermod -l <new_username> -d /home/<new_username> -m <original_user_name>`
	- change username for a user
	- home directory must be specified so that old directory contents can be copied to it


- `su <username>`
	- temporarily switch the user ID, primary group ID and home directory
- `su - <username>`
	- temporarily log on as another user and user their profile