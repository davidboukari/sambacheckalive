# sambacheckalive

On ubuntu 18/04 LTS samba-service crash every days

### Credentials
```
cat ~/smbclient.conf
username=Administrator
password=xxxxxx
domain=mydomain
```

### samba_check.sh
```bash
cat samba_check.sh
#!/bin/bash

SERVICE=samba-ad-dc.service

smbclient -A ~/smbclient.conf -L localhost
returnCode=$?

if [ $returnCode -ne 0 ];then
  /usr/bin/logger -p local0.info -t samba_check.sh  "stop ${SERVICE}"
  /bin/systemctl stop $SERVICE
  /usr/bin/logger -p local0.info -t samba_check.sh  "start ${SERVICE}"
  /bin/systemctl start $SERVICE
fi
```

### Install crontab
```
2 *  *   *   *     /root/owncloud/samba_check.sh
```
