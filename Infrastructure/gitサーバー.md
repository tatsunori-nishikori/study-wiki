# gitサーバ

## インストール

```console
# yum install -y git git-daemon git-all xinted
```


service git

```sh
{
  disable = no
  port = 9418
  socket_type = stream
  wait = no
  user = nobody
  server = /usr/bin/git-daemon
  server_args = --inetd --export-all --base-path=/var/git --enable=receive-pack
  log_on_failure += USERID
}
```
