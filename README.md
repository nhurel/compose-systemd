# compose-systemd

Tiny go program to generated Podman systemd file from a docker-compose.yaml file.

Podman can run containers as standard systemd services. It even provides a way to generate the systemd files but this requires to create the containers first, and have them attached to a pod.
This is not possible to do it from a compose file.

This program helps fill the gap by generating directly the systemd files from a compose file. 
The compose service names are prefixed by the project name, ie if your wordpress compose file has a service named "db" it will translate to a wordpress-db.service systemd unit.
All environment variables are also defined as `Environment` variables in the unit, so you can then override them via standard systemd options. 

Once the files are generated, you only need to :
- copy them all under `/etc/system/systemd/`
- `systemctl daemon-reload`
- `systemctl enable APPNAME-pod.service APPNAME-app.service APPNAME-db.service`
- `systemctl start APPNAME-pod.service`