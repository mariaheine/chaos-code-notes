
# Episodes

1. asd
2. sad
3. asd
4. [Intro to CLI](https://www.youtube.com/watch?v=IYbtai7Nu2g)
	1. [[#Connecting via `Console port`]]
	2. [[#Cisco IOS CLI]]

# Configuring Cisco Devices

## Connecting via `Console port`

- USB Mini-B
- or
- RJ45
	- RJ45 -> DB9
	- Cable used is `Rollover Cable`
	- Pins are crossed
		- 1-8
		- 2-7
		- 4-5
		- 5-4
		- 6-3
		- etc.
- Accessing `CLI` with PuTTY
	- Use `Serial` connection type.
	- Default setting should work.
	- But:
		- *Speed (baud) 9600 bps*
		- *Data bits: 8*
		- *Stop bits: 1* (for each 8 bits of data, 1 stop bit is sent to mark the end of the bits)
		- Parity *no*
		- Flow Control *no*

## Cisco IOS CLI

***user exec mode***
- `hostname>` ***user exec mode***
- `enable` enter ***privileged user mode***

***privileged exec mode***
- `hostname#` 
	- `#` denotes privileged user mode
- `en`
	- `?` show available commands
		- different for different exec modes!
	- `e?`
		- will show commands beginning with e
	- `enable ?`
		- will show allowed parameters for `enable`
- `configure terminal`
	- `conf t`
	- will put you in ***global configuration mode***

***global config mode***
- `hostname(config)#`
- `enable password PASSWORD`
	- will set unencrypted password
	- `enable PASSWORD ?`
		- will show options for the password, among them `LINE` in caps lock, this means you don't type in LINE but a value that is supposed to replace it
	- `enable service password-encryption`
		- equivalent to `enable 7 PASSWORD`
		- without this password is shown fully unencrypted when showing given config
	- `enable 5 PASSWORD`
		- number determines type of encryption used
		- `7` means proprietary cisco encryption *which is weak as fuck*
		- `5` uses MDA5 encryption which is much stronger
- `exit`
	- will quit to priv. exec mode
- `show running-config`
	- `sh run`
	- running config is the one you are currently editing in your session
	- it won't be automatically saved and used on startup! to save use one of:
		- `write`
		- `write memory`
		- `copy running-config startup-config`
- `show startup-config`
	- `sh sta`
	- the config loaded on startup
- `enable secret SECRET`
	- encrypted by default as opposite to password
	- actually cannot be not encrypted
	- stronger encryption than password
	- *if both secret and password are set, secret will always take precedence when trying to log in!*
- `do PRIVILEGED-COMMAND`
	- lets you run exec commands while in config mode
	- like `do sh run`
		- to lookup running config while in config mode
- `no COMMAND`
	- undo past commands `for future commands`
	- `no service password-encryption`
		- will remove password encryption for future password commands
