# Changelog

## 1.1.0 (26 October 2026)

**New: network roles.** Each network card has a role: Everyday + downloads, Everyday
only, Downloads only, or Not used. For example, keep Ethernet for browsing, games and
calls and use a phone hotspot only for downloads.

- Downloads from boosted apps and sites, and the built-in downloader, use only networks
  whose role allows downloads. If the download network drops, they wait for it instead
  of moving to your everyday network.
- The main network and fail-over use only networks that allow everyday traffic.
- Settings > Networks has the same role menu. The old "Use" switch is replaced: on
  becomes Everyday + downloads, off becomes Everyday only.
- Site lists (Boost sites, Always main) warn when an app name such as `chrome.exe` is
  typed, since they only take site names.

**Fixed**

- Websites hung or loaded very slowly in Combined mode on PCs without IPv6, because
  NetFuse offered IPv6 that couldn't go anywhere. NetFuse now leaves IPv6 alone unless a
  network for everyday traffic really has it.
- Chrome slowed down to a crawl while another download was running. QUIC (HTTP/3)
  connections could end on a harmless network error, and short bursts were dropped.
- "Stop at the data limit" now really stops the main network once its allowance is used.
- The speed chart's labels were cut off and used odd steps.

## 1.0.0 (22 October 2026)

First public release, for Windows 10 and 11 (x64).

- Combined mode: chosen apps and sites are split across every network; everything else
  stays on the main network.
- Rules: boost apps, boost sites and always-main sites, presets for Steam, Epic and
  torrent clients, and a box to test which rule a connection would match.
- Built-in downloader: one file over all networks in blocks, with resume, Normal and
  Aggressive stream modes, a limit on connections per server, and Fix link for expired
  links.
- Dashboard with live speed per network, Connections screen, per-network speed and data
  limits, and Slow mode.
- Tray icon, start with Windows, and Combined mode turned off cleanly on quit, sign-out,
  shutdown or after a crash.
- Restore network in Settings and in the tray menu.
