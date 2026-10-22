# Privacy

NetFuse has no accounts, no telemetry, no advertising, no crash reporting and no
automatic update checks. It sends nothing about you or how you use it to the developer
or to anyone else.

## What stays on your computer

Everything NetFuse stores is in `%APPDATA%\NetFuse`:

| File | Contents |
|---|---|
| `settings.json` | Your settings: rules, network names and colors, limits |
| `downloads.json` | Your download list: links, file names, sizes, progress |
| `usage.json` | Data used per network per day, for the data limits |
| `logs\netfuse.log` | Start and stop times, errors and setting changes. At the default level it contains no links, sites or traffic. |
| `recovery.json` | Exists only while Combined mode is on; used to clean up after a crash |

Downloaded files go to the folder you choose (by default your Downloads folder).

## Network connections NetFuse makes by itself

- **Health checks:** every 10 seconds, through each network, a connection to
  `1.1.1.1:443` (Cloudflare) and `8.8.8.8:443` (Google) to measure whether the network
  works and its latency. No data is sent; the connection is closed straight away. You
  can change these servers in `settings.json` (`health_targets`).
- **Name lookups for downloads:** sent to your main network's DNS servers, or to
  `1.1.1.1` / `8.8.8.8` if that network has none.
- **Downloads:** only to the servers in the links you add.

## Traffic in Combined mode

While Combined mode is on, your internet traffic passes through NetFuse on your
computer, which sends each connection out over one of your networks. To apply your
rules, NetFuse looks at which app opened a connection, its destination address and
port, and the site name at its start (TLS server name, HTTP Host header or an earlier
DNS answer). This happens in memory only. Traffic isn't stored, logged at the default
level, changed or decrypted.

The Connections screen shows the busiest current connections. That list is kept in
memory and is gone when NetFuse quits.

## Removing your data

Uninstall NetFuse from Windows Settings > Apps, then delete the folder
`%APPDATA%\NetFuse`.
