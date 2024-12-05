### Bash Profiles
- `/etc/profile` and `/etc/bashrc` are system-wide
	- `~/.bashrc` , `~/.bash_profile` are user specific
- `profile` ones are used for env variables
- `bashrc` ones are used for bash shell features like aliases

#### Login Script Order
1. `/etc/profile`
	- read once at login
2. `~/.bash_profile`
	- read once at login
3. `~/.bashrc`
4. `/etc/bashrc`