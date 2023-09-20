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

