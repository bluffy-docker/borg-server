
minimal borgserver

https://github.com/bluffy-docker/borg-server
# install

Copy docker-compose
```
cp docker-compose.example.yml docker-compose.yml
```
create dirs und permissions
```
mkdir -p data/{.ssh,backups}
chown 1000:1000 -R data
```
run 
```
docker-compose up
```

# config

add authorizations

```
vi data/.ssh/authorized_keys
```
or with sudo if permissions denied
```
sudo vi data/.ssh/authorized_keys
```
insert examples:

```
command="borg serve --restrict-to-path /backups --append-only" ssh-rsa AAAAB3NzaC1yc....5Bq9HVUj1q24sSkKLQ== user@localhost.de
command="borg serve --restrict-to-path /backups" ssh-rsa AAAAB3NzaC1yc....5Bq9HVUj1q24sSkKLQ== admin@localhost.de
command="borg serve --restrict-to-path /backups/repo1 --append-only" ssh-rsa AAAAB3NzaC1yc....5Bq9HVUj1q24sSkKLQ== user1-repo1@localhost.de
```

# Example
## Create Repo 

set variables
```
export BORG_PASSCOMMAND='pass show backup'
export BORG_PASSPHRASE='test1'
```
create repo
```
borg init --encryption=repokey  ssh://borg@localhost:2022/backups/reop1
```

## Run backup

set variable
```
export BORG_REPO=ssh://borg@localhost:2022/backups/reop1
```
run repo
```
borg create                         \
    --stats                         \
    --show-rc                       \
    --info                          \
    --compression lz4               \
    --exclude-caches                \
    --exclude '/home/*/.cache/*'    \
    --exclude '/root/*/.cache/*'    \
    --exclude '/var/cache/*'        \
    --exclude '/var/tmp/*'          \
    --exclude '/var/lib/*'          \
                                    \
    ::'{hostname}-{now}'            \
    /etc                            \
    /home                           \
    /root                           \
    /var                            \
```

```
borg list
```



