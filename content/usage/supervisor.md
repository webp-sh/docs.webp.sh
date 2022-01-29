# Supervisor

To keep WebP Server running, you can use a various of supervisors.

## screen or tmux
Use `screen` or `tmux` to avoid being terminated. Let's take `screen` for example
```sh
screen -S webp
./webp-server --config /path/to/config.json
```
(Use Ctrl-A-D to detach the `screen` with `webp-server` running.)

## Systemd
Don't worry, we've got you covered!

The standard systemd service file will show on your screen. You may want to use `>` to redirect to a file.

```sh
./webp-server -dump-systemd
```

Download `webp-server` to `/opt/webps/webp-server`, and create a config file to `/opt/webps/config.json`, then,

```sh
./webp-server -dump-systemd > /lib/systemd/system/webp-server.service
systemctl daemon-reload
systemctl enable webp-server.service
systemctl start webp-server.service
```