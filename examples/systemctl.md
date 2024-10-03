# systemctl

systemctl start <service> 	Starts the specified service.
systemctl stop <service> 	Stops the specified service.
systemctl restart <service> 	Restarts the specified service.
systemctl enable <service> 	Enables the specified service to start automatically at boot.
systemctl disable <service> 	Disables the specified service from starting automatically at boot.
systemctl status <service> 	Displays the status of the specified service (e.g., Active, Inactive, Failed).

systemctl list-units --all --type=service


systemctl list-units --type=service --state=running


systemctl status <service> config files locate /etc/systemd/system/