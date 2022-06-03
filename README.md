# pgweb

[pgweb](https://github.com/sosedoff/pgweb) is a web-based browser for postgresql.

This documentation covers installation and setting up on an amazon ec2 ubuntu 20.04.4 LTS machine.

## Installation
Update your system packages
``` sudo apt update && sudo apt upgrade -y ```

Download and install pgweb 
```curl -s https://api.github.com/repos/sosedoff/pgweb/releases/latest \
  | grep linux_amd64.zip \
  | grep download \
  | cut -d '"' -f 4 \
  | wget -qi - \
  && unzip pgweb_linux_amd64.zip \
  && rm pgweb_linux_amd64.zip \
  && mv pgweb_linux_amd64 /usr/local/bin/pgweb ```

Check installation by running 
```pgweb -v
   
   Output
   Pgweb v0.11.11 (git: db2a7a8aa5bc449e4efa78cada9c76c3fe33bc39) (go: go1.17.6) (build time: 2022-03-30T04:36:12Z) ```

## Usage
Start server with `pgweb` 

By default pgweb runs on 127.0.0.1, localhost on port 8081. For amazon ec2, bind 0.0.0.0 to be able to access pgweb via the instance's public IP/DNS. ```pgweb --bind 0.0.0.0```  

Access pgweb via *http://<instance-ip>:8081*

To connect to a database at start of pgweb you can add the connection flags;
```pgweb --host db-host-endpoint --user dbuser --db dbname```

pgweb also supports url scheme connection;
```pgweb --url postgres://dbuser:password@host:port/database```

See more [CLI options/flags](http://radio.garden/listen/voice-of-america-africa/3-yMRDpo).

## Extend into a service
This lets systemd manage pgweb and can be monitored via `systemctl`.

Copy/move the file `pgweb.service` to `/etc/systemd/system` folder.
``` sudo mv pgweb.service /etc/systemd/system```

Reload services to include the new service.
```sudo systemctl daemon-reload```

Start the service and check the status.
```sudo systemctl start pgweb.service
sudo systemctl status pgweb.service```

If all goes through, enable the service to start at boot
```sudo systemctl enable pgweb.service```

## FAQ 
