## Basic usage example

```bash
./target/release/bluestation-bs ./config.toml
```

For improved scheduling stability (lower jitter), you can run the process with a real-time priority using chrt:
```bash
chrt -f 73 ./target/release/bluestation-bs ./config.toml
```
Note: Real-time scheduling typically requires elevated privileges and appropriate system limits/capabilities.

## Running as a service

A sample systemd service file is provided in `contrib/systemd/bluestation-bs.service`. It runs the process with real-time scheduling priority (equivalent to `chrt -f 73`) and restarts it automatically on failure.

Edit the file to match your setup before installing. The three values to adapt are:

- `User` and `Group`: the user account that will run the service
- `WorkingDirectory`: path to the `tetra-bluestation` folder
- `ExecStart`: full paths to the binary and your `config.toml`

Then install and enable the service:
```bash
sudo cp contrib/systemd/bluestation-bs.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable bluestation-bs
sudo systemctl start bluestation-bs
```

Common management commands:
```bash
systemctl status bluestation-bs        # show current state
systemctl stop bluestation-bs          # stop the service
systemctl disable bluestation-bs       # disable start at boot
journalctl -u bluestation-bs --output=cat -f  # follow the log
```


## Logging events
In the config file, uncomment this line:

```toml
debug_log = "./verbose_log.txt"
```

## Filtering out some console messages
TODO: re-write and explain the usage.

Append `| grep -v "phy\|common\|lmac"` to filter out some logs.