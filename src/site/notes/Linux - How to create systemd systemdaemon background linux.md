---
{"dg-publish":true,"permalink":"/linux-how-to-create-systemd-systemdaemon-background-linux/"}
---

## Step 1:

Find your user defined services. Ubuntu was at `/etc/systemd/system/`

## Step 2:

Create a text file with your favorite text editor name it `whatever_you_want.service`

## Step 3:

Put following **Template** to the file `whatever_you_want.service`





```sh
[Unit]
Description=Metabase Daemon

[Service]
PIDFile=/var/tmp/metabase.pid
WorkingDirectory=/usr/local/bin/metabase/
User=ubuntu
Group=ubuntu

#RUN COMMAND HERE
ExecStart=/usr/bin/java -jar -Xmx512m /usr/local/bin/metabase/metabase.jar 

Restart=on-failure
RestartSec=30
PrivateTmp=true

StandardOutput=metabase
StandardError=metabase_err
SyslogIdentifier=metabase

[Install]
WantedBy=multi-user.target
```



## Step 4:

**Run your service**
*as super user*

```sh
$ systemctl start whatever_you_want.service # starts the service
$ systemctl enable whatever_you_want.service # auto starts the service
$ systemctl disable whatever_you_want.service # stops autostart
$ systemctl stop whatever_you_want.service # stops the service
$ systemctl restart whatever_you_want.service # restarts the service
$ journalctl -e -u metabase.service # to look Outputs
```

