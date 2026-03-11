> common command line commands
- https://kopia.io/docs/reference/command-line/common/

> create a local repository
- `$ kopia repository create filesystem --path=...`
- https://kopia.io/docs/reference/command-line/common/repository-create-filesystem/

> actions
> https://kopia.io/docs/advanced/actions/

Nice example is `Dumping SQL databases before snapshotting`
- This script invokes `mysqldump` to create a file called `dump.sql` in the directory that’s being snapshotted. 
- This can be used to automate database backups.
- Actions can be run on different triggers like before or after a snapshot
```
#!/bin/sh
set -e
mysqldump SomeDatabase --result-file=$KOPIA_SOURCE_PATH/dump.sql
```

> connect to a local repository
> https://kopia.io/docs/reference/command-line/common/repository-connect-filesystem/

- `$ kopia repository connect <provider> <flags>`
	- in our case provider is just `filesystem`
- `kopia repository connect filesystem --path="/media/merryangel/Crucial X8/kopia"`
	- i am not certain how it worked, it asked me for a password and then it accepted me
	- was it because a user i made with `kopia server users add` was `maria@lain`?


> status
> https://kopia.io/docs/reference/command-line/common/repository-status/

- `kopia repository status`
- `-t`
	- will display reconnect command
- `-s`
	- will insecurely include password in `-t` command token

> snapshots

- `snapshot list`
	- https://kopia.io/docs/reference/command-line/common/snapshot-list/
	- `kopia snapshot list --all --human-readable --owner` from here you can grab the hash of a snapshot you are interested in mounting
- `snapshot create`
	- https://kopia.io/docs/reference/command-line/common/snapshot-create/
- `snapshot restore`
	- https://kopia.io/docs/reference/command-line/common/snapshot-restore/
	- remember to create a specific directory you want it to be 'unpacked' into, since if the original container directory was like 'repositories' it will not be restored but its contents
	- example:
		- `kopia snapshot restore k1a574df197b1a611991bc569cc11858d /home/merryangel/Projects/repositories`
		- where the hash you got from `snapshot list`

> mounting

- `kopia mount kac3840a987d957f9320304488eb7ee81 ~/Desktop/snc_mount` 
	- use the hash and create a dir for the mount to take place

> config
> https://kopia.io/docs/reference/command-line/#configuration-file

- `--config-file` is a global flag that can be applied to all kopia commands
	- it will override the default configuration file location
	- `kopia --config-file=~/.config/kopia/repository.config snapshot list`
	- `kopia --config-file=~/.config/kopia/repository.config repository status`
- an example config file
```json
λ cat ~/.config/kopia/repository.config  
{  
 "storage": {  
   "type": "filesystem",  
   "config": {  
     "path": "/media/merryangel/Crucial X8/kopia",  
     "fileMode": 384,  
     "dirMode": 448,  
     "dirShards": null  
   }  
 },  
 "caching": {  
   "cacheDirectory": "../../.cache/kopia/be0941717132f1de",  
   "maxCacheSize": 5242880000,  
   "maxMetadataCacheSize": 5242880000,  
   "maxListCacheDuration": 30  
 },  
 "hostname": "lain",  
 "username": "merryangel",  
 "description": "Repository in Filesystem: /media/merryangel/Crucial X8/kopia",  
 "enableActions": false,  
 "formatBlobCacheDuration": 900000000000  
}%
```
todo
```
2523  vim .kopiaignore  
2524  kopia snapshot estimate /home/merryangel/Projects/sanctuary/\n  
2525  kopia snapshot create /home/merryangel/Projects/sanctuary/\n  
2526  kopia snapshot list --all --human-readable --owner
```
