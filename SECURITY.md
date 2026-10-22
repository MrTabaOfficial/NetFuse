# Security policy

NetFuse runs with administrator rights and handles your network traffic, so security
reports are taken seriously.

## Supported versions

| Version | Security fixes |
|---|---|
| 1.0.x (latest) | Yes |
| Older | No: please update first |

## Reporting a vulnerability

**Please don't open a public issue for security problems.**

Report it privately on GitHub: open the **Security** tab of this repository, then
**Report a vulnerability**, or go straight to
<https://github.com/MrTabaOfficial/NetFuse/security/advisories/new>. Only the maintainer
can see the report.

Please include:

- the NetFuse version (Settings > About) and your Windows version;
- what an attacker could do, and what they would need (local access, same network,
  a malicious website, and so on);
- steps to reproduce, or a proof of concept;
- any logs that help (Settings > Logs). Remove anything private first.

What happens next:

- You get an acknowledgement, usually within a week.
- The problem is confirmed, fixed and released, and you're kept informed along the way.
- Once a fix is available, the issue is published in a security advisory. You're
  credited there unless you prefer not to be.

Please give a reasonable amount of time to fix the problem before you disclose it
publicly. Good-faith research that follows this policy is welcome. Don't access other
people's data or disrupt networks or services that aren't yours.

## What NetFuse does with administrator rights

So you can judge the risk for yourself:

- **Combined mode** creates a virtual network adapter called `NetFuse` (the Wintun
  driver) and adds two routes, `0.0.0.0/1` and `128.0.0.0/1`. It doesn't change your
  default routes, your adapters or their DNS settings, or the firewall (DNS is set only
  on NetFuse's own adapter, which disappears when Combined mode stops). It removes the routes when you
  turn it off, when NetFuse quits, when Windows signs out or shuts down, and on the
  next start after a crash.
- **App detection** reads Windows' table of open connections to find which program
  owns each one, so rules like "Boost steam.exe" work. It doesn't read other
  programs' memory or files.
- **Rules by site** read the site name from the start of a connection (the TLS
  "server name", the HTTP Host header, or an earlier DNS answer). Traffic is passed on
  unchanged and isn't stored. NetFuse doesn't decrypt HTTPS.
- **Start with Windows** creates a Task Scheduler task named `NetFuse` that starts the
  app at sign-in. Turning the setting off deletes the task.
- **No listening ports.** NetFuse doesn't accept connections from the network.

## Verifying downloads

- Each release lists the SHA256 checksum of every file in `SHA256SUMS.txt`. Check your
  download before installing:
  ```powershell
  Get-FileHash .\NetFuse_1.0.0_x64-setup.exe -Algorithm SHA256
  ```
- The installers aren't code-signed yet. Only download NetFuse from this repository's
  [Releases page](https://github.com/MrTabaOfficial/NetFuse/releases).
- The bundled `wintun.dll` is the unmodified official build, signed by its publisher.

## Data and privacy

NetFuse has no telemetry and sends nothing to the developer. See [PRIVACY.md](PRIVACY.md).
