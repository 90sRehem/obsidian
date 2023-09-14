#linux #programming #git 

1. generate ssh:
```bash
ssh-keygen -C "email@example.com"
```
2. list ssh-keys:
```bash
 ls -al ~.ssh
```
3. remember the passphrase
```bash
eval `ssh-agent`
```