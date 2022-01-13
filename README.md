
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

# Create Repo Example

```
borg init --encryption=repokey  ssh://borg@localhost:2022/backups/reop1
```

