## Basic usage example

```bash
./target/release/tetra-bluestation ./config.toml
```

For improved scheduling stability (lower jitter), you can run the process with a real-time priority using chrt:
```bash
chrt 99 ./target/release/tetra-bluestation ./config.toml
```
Note: Real-time scheduling typically requires elevated privileges and appropriate system limits/capabilities.

## Filtering out some logs
TODO: re-write and explain the usage.
Append `| grep -v "phy\|common\|lmac"` to filter out some logs.