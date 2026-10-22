# NetFuse user guide

## Contents

- [First start](#first-start)
- [Combined mode](#combined-mode)
- [Rules](#rules)
- [Connections](#connections)
- [Downloads](#downloads)
- [Settings](#settings)
- [Tray icon and starting with Windows](#tray-icon-and-starting-with-windows)
- [Restoring your network](#restoring-your-network)
- [Questions](#questions)
- [Uninstalling](#uninstalling)

## First start

NetFuse asks for administrator rights when it starts, because Combined mode creates a
network adapter and adds routes. The **Dashboard** then shows a card for every
connected network that has internet access: Ethernet, Wi-Fi, a phone over USB or Wi-Fi.

On each card:

- **★** marks the main network. By default that's the network Windows uses. Click the
  star on another card to choose a different one.
- The **pencil** renames the network and changes its color.
- **Use** includes or excludes the network from splitting.
- Live download and upload speed, latency, and data used today.

A network shows **No internet** when NetFuse's health check can't get through it, and
then nothing is split onto it. A phone hotspot without mobile data looks connected in
Windows but shows up here as having no internet.

## Combined mode

Turn it on with the big switch on the Dashboard, or from the tray menu.

- Connections from **boost apps** and to **boost sites** are split across all usable
  networks. Each new connection goes to whichever network is fastest at that moment.
- Everything else uses the **main network**, as without NetFuse.
- Addresses on a network's own local network (your router, a NAS, a printer) always
  go through that network.

Turn Combined mode off with the same switch. NetFuse removes its routes and the
adapter, and your network works exactly as before.

## Rules

The **Rules** screen has three lists:

| List | Example | Effect |
|---|---|---|
| Boost apps | `steam.exe` | Every connection from this program is split |
| Boost sites | `*.steamcontent.com` | Connections to these sites are split, whatever app opens them |
| Always main | `login.example.com` | Always the main network; beats every other rule |

- `example.com` and `*.example.com` both include all subdomains. Write `=host.example.com`
  to match only that exact host.
- **Presets** add ready-made rules for Steam, Epic Games and torrent clients.
- **Test a connection** shows which rule would decide for an app, site and port.

Rules are checked in this order, and the first match decides:

1. The site is in **Always main** → main network.
2. It's UDP and **Split UDP** is off → main network.
3. The port isn't one that may be split → main network.
4. The app is in **Boost apps** → split.
5. The site is in **Boost sites** → split.
6. Otherwise → main network.

**Ports.** By default only ports 80 and 443 (web downloads) may be split. Other ports
carry games, calls, VPNs and logins, which can break when their connections come from
different addresses. Click a port range on the Rules screen to turn it on or off. The
Steam range 27015–27050 is listed but off; the Torrent preset adds 1024–65535.

**Split UDP.** Off by default. Turn it on for torrent clients; each UDP flow still stays
on one network.

## Connections

While Combined mode is on, **Connections** lists the busiest open connections: app,
site, network, route (Main or Split), speed and total. Filter by app or site, or show
only Main or only Split.

Hover a row for quick rules:

- ⚡ **Boost this app**
- 🛡 **Always main for this site**

A connection whose app shows as *unknown* closed before NetFuse could look it up. It
uses the main network.

## Downloads

**New download** takes a link (filled in from the clipboard when it holds one), a
folder, and a stream mode:

| Mode | Streams per network | Use it when |
|---|---|---|
| Normal | starts at 4, grows to 16 while speed rises | almost always |
| Aggressive | starts at 8, grows to 32 | fast connections and servers that allow many connections |
| Fixed | exactly the number you set | testing or special servers |

Normal mode also opens at most 16 connections to one server, over all networks and
downloads together, so servers aren't overloaded.

- Downloads are grouped as Downloading, Queued, Paused, Needs attention and Finished.
- Click a download for details: speed per network, time left, streams, and a block map
  colored by the network that fetched each part of the file.
- **Pause** and **Resume** keep the progress, also across restarts.
- **Fix link**: when a link expires (common with signed download links), paste a fresh
  link to the same file and the download continues where it was.
- If the file on the server changed, NetFuse won't mix old and new parts. The download
  stops with "file changed on server".
- A server that doesn't support partial downloads gets a single stream over the main
  network.

## Settings

- **Main network:** Automatic (follows Windows) or a fixed network.
- **Fail-over:** if the main network goes down, main traffic moves to another network.
- **Per network:** a download speed limit and a data allowance per day, week or month.
  A network that has used its allowance is no longer used for splitting or downloads,
  and its card shows **Data limit**. With **Stop at the data limit** on, a main network
  over its allowance also stops carrying everyday traffic in Combined mode, and another
  network with allowance left takes over.
- **Total download limit** and **Slow mode** (2 MB/s for everything, for example while
  someone else needs the connection).
- **Downloads:** folder, how many run at once, stream mode, connections per server.
- **Theme:** System, Dark or Light.
- **About:** version, license and third-party notices.
- **Restore network:** see below.

## Tray icon and starting with Windows

Closing the window keeps NetFuse running in the tray, so Combined mode and downloads
continue. The tray menu has **Show NetFuse**, **Combined mode**, **Slow mode**,
**Restore network** and **Quit NetFuse**. Quit turns Combined mode off before NetFuse
exits.

**Start with Windows** (Settings > App) starts NetFuse in the tray when you sign in.
Because NetFuse runs as administrator, Windows won't start it from the normal startup
list, so it uses a Task Scheduler task named `NetFuse` instead. You won't see it under
Startup apps. Turning the setting off deletes the task.

## Restoring your network

If the internet stops working, try these in order:

1. Turn Combined mode off.
2. **Settings > Restore network**, or **Restore network** in the tray menu.
3. Quit NetFuse from the tray.
4. In an administrator PowerShell:
   ```powershell
   Get-NetRoute -InterfaceAlias NetFuse -ErrorAction SilentlyContinue | Remove-NetRoute -Confirm:$false
   ```
5. Restart the computer.

NetFuse never changes your adapters' own settings, DNS or firewall.

## Questions

**Can one download from a browser or game use both networks at once?**
A single connection always uses one network, because the server sees its address. Apps
that open many connections (Steam, most game launchers, torrent clients) get faster in
Combined mode because their connections are spread. For a single big file, use the
built-in downloader, which splits the file itself.

**Why doesn't my speed add up?**
Two adapters connected to the same router share the same internet line. The networks
must be really separate, such as home internet plus a phone.

**My phone hotspot shows "No internet".**
The phone is connected but isn't sharing mobile data. Check that mobile data is on and
that a website opens on the phone. Wi-Fi to the phone is often more reliable than USB.

**Does NetFuse work with a VPN?**
If a VPN holds the default route, NetFuse won't start Combined mode. Turn the VPN off
first. The downloader works either way.

**Windows says "Windows protected your PC" or blocks the installer.**
The installer isn't code-signed yet. Click **More info**, then **Run anyway**. With
Smart App Control on, Windows blocks unsigned apps completely.

## Uninstalling

Uninstall NetFuse from Windows Settings > Apps. To remove your settings, download list
and logs too, delete `%APPDATA%\NetFuse`.
