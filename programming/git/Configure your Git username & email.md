#git #programming 

### To set your global username/email configuration:

1. Open the command line.
    
2. Set your username:  
```bash
	git config --global user.name "FIRST_NAME LAST_NAME"
```
    
3. Set your email address:  
   ```bash
	git config --global user.email "MY_NAME@example.com"
```

### To set repository-specific username/email configuration:

1. From the command line, change into the repository directory.
    
2. Set your username:  
    ```bash
git config user.name "FIRST_NAME LAST_NAME"
```
    
3. Set your email address:  
```bash
    git config user.email "MY_NAME@example.com"
```
    
4. Verify your configuration by displaying your configuration file:  
```bash
    cat .git/config
```