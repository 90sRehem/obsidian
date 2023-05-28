#terminal #linux

list ports:

```shell
sudo lsof -i -P -n | grep LISTEN 
```

```bash
sudo kill -9 `sudo lsof -t -i:9001`
```

If that doesn't work, you could also use `$()` for command interpolation:

```bash
sudo kill -9 $(sudo lsof -t -i:9001)
```