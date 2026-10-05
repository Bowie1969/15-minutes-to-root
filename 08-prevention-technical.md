# Recommended prevention — technical

Controls below are mapped to the findings in `04-report-technical.md` §6.
Priority is set by the ratio of defensive leverage to deployment cost on the
target population this run represents (SMB and prosumer, not enterprise).
Enterprise-grade variants are called out where they diverge.

## 1. Immediate, no-cost (defeats this exact run)

### 1.1 Credential hygiene on the shell — defuses **F1**
- Set `HISTCONTROL=ignoreboth` and `HISTIGNORE='sudo *:*password*:* -p *:* --password*'`
  in `~/.bashrc`.
- Prefer `zsh` with `setopt HIST_IGNORE_SPACE` and alias `sudo='sudo '` so every
  sudo invocation is space-prefixed and silently excluded from history.
- Audit: `grep -nE '(password|passwd|^\s*[A-Z][a-z]+[0-9]{4}$)' ~/.bash_history`.
  If anything matches, `shred` the file and rotate the password.
- Prefer `pass`, `bitwarden-cli`, or an OS keychain over any pattern that puts a
  secret on a shell line.
- Enterprise variant: enforce via MDM-managed `/etc/skel/.bashrc`, audit via
  osquery (`SELECT * FROM shell_history WHERE command LIKE '%sudo%' AND
  command_line LIKE '%<password-pattern>%'`).

### 1.2 Screen lock + USB device policy — defuses **F2**
- `gnome-session`/KDE: idle lock ≤ 60 s.
- Linux: install USBGuard, generate initial policy with
  `usbguard generate-policy > /etc/usbguard/rules.conf`, then reject new devices
  by default: `usbguard set-parameter InsertedDevicePolicy=reject`.
- Windows: GPO `Computer Config → Admin Templates → System → Device Installation
  Restrictions`, prevent new HID without admin, or deploy BeyondTrust / Ivanti
  device control.
- macOS: enable "Require password immediately after sleep", pair with Jamf-managed
  System Extension to restrict HID.
- Physical: USB port blockers on exposed devices (desktop at kiosk, reception
  front-of-house machine).

### 1.3 Terminate the beacon pattern via EDR — defuses **F3**, **F7**
- Any paid EDR (CrowdStrike, SentinelOne, Defender for Endpoint) will flag
  `curl | sh/bash` and short-lived `python3 /tmp/.bd` child-of-browser-or-shell
  as a parent–child anomaly.
- Free equivalent: `auditd` + a tiny rule set:
  ```
  -a always,exit -F arch=b64 -S execve -F path=/usr/bin/curl -F key=curl_exec
  -a always,exit -F arch=b64 -S execve -F path=/tmp -F key=tmp_exec
  ```
  Ship logs to a central host; alert on `tmp_exec` plus any child under `/tmp`.
- Sysmon equivalent on Windows: ProcessCreate for `curl.exe`/`powershell.exe`
  with `-c` or `-w hidden`, FileCreate under `%TEMP%`.

## 2. Short-term, low-cost (defeats the capability class)

### 2.1 Wi-Fi segmentation + unknown-device alert — defuses **F8**
- Separate SSID (plus VLAN tag) for workstations, guest, and IoT. 802.1Q
  trunk from the AP to the gateway; dropped at the firewall between VLANs
  except explicit allow (e.g. workstation → printer only on TCP 9100).
- On the workstation VLAN, deny broadcast domain membership for anything
  that's not on a MAC allowlist (static DHCP reservations + 802.1X where
  supported).
- Daily alerting: `arp-scan -lq | diff - /etc/known-macs` on a cron;
  pipe mismatch to the operator's phone (Pushover, Signal, pager).
- Consumer-grade router minima: enable the "new device joined" push alert
  (ASUS, Google, Eero all have it); use the guest SSID for anything that
  isn't a known business device.

### 2.2 Patch management — defuses **F5**, **F6**
- `unattended-upgrades` on Debian/Ubuntu, dnf-automatic on RHEL/Fedora,
  WSUS or Windows Update for Business on Windows.
- Specifically: patch `polkit >= 0.120-0ubuntu0.22.04.1` (CVE-2021-4034),
  patch `redis >= 7.0` AND bind to `127.0.0.1` with `requirepass` set.
- Audit cadence: monthly `apt list --upgradable | wc -l` → ticket if > 0.

### 2.3 Sudo hardening — defuses **F4**
- `Defaults use_pty, logfile=/var/log/sudo.log, lecture=always, passwd_tries=1,
  timestamp_timeout=0` in `/etc/sudoers`.
  (`passwd_tries=1` + `timestamp_timeout=0` means every sudo requires fresh
  authentication, and one wrong guess is logged — hostile automation gets one
  shot per invocation.)
- Where possible, move to passwordless sudo for a tightly scoped allowlist
  of commands and *remove* the user from the broad `sudo` group entirely —
  the opposite of F4's current state.

## 3. Medium-term, moderate-cost (defence-in-depth)

### 3.1 Egress + lateral visibility
- A single Zeek sensor on a mirror port of the gateway writes `conn.log`,
  `http.log`, `dns.log`. Even unparsed, these feed into any lightweight SIEM
  (Elastic, Wazuh, Grafana + Loki).
- Alert on: *any* HTTP request from a workstation to an RFC1918 non-gateway
  address (this run's `10.10.66.175 → 10.10.66.5:8080` pattern would have
  tripped on the first poll).
- Alert on: short-connection beaconing — >50 conns/5 min from one source to
  one destination with low average bytes-per-conn. The beacon's 5 s poll
  profile is textbook for this detector.

### 3.2 Host EDR with behavioural rules
- Deploy Wazuh (free) or a commercial EDR to every endpoint, not just
  servers.
- Minimum ruleset: parent–child chain curl/wget → shell, writes to `/tmp/.*`
  that are then executed, python/perl/ruby invoked without a `.py`/`.pl`
  argument file.
- Specifically for the AI operator class: alert on sudden bursts of `exec`
  activity after a long idle period (the beacon's job-queue pattern
  manifests as a 60-second shell burst every few minutes).

### 3.3 Credential vaulting + MFA on local sudo
- Pair accounts with Yubikey PAM: `pam_u2f` for `sudo` means possession-based
  second factor on every escalation. A stolen `.bash_history` password is
  then useless without the hardware token.
- Or `pam_oath` with a TOTP generator on the user's phone.
- Audit by `grep -c pam_u2f /etc/pam.d/sudo` on every endpoint.

## 4. Operational hygiene (defeats the human-habit root cause)

- **Monthly:** review `.bash_history`, `.zsh_history`, `~/.config/*/history*`
  for anything that looks like a credential or token. Rotate on hit.
- **Quarterly:** rotate sudo-privileged user passwords, Wi-Fi PSK, and router
  admin credentials.
- **Per staff change:** rotate Wi-Fi PSK, revoke MDM profile, retire the user's
  SSH keys everywhere.
- **Per device-of-unknown-origin found on the premises:** powered-off removal,
  photographic record, chain-of-custody bag, do not plug it into anything.

## 5. If this run had already happened — the response

1. Isolate the host (unplug Ethernet, airplane-mode the Wi-Fi).
2. From a different device, rotate every credential the compromised user had
   access to — email, cloud, VPN, SaaS. Treat `sudo` password as "the key to
   every service that user has signed in to on that host."
3. Collect: `/tmp/.bd`, `/tmp/*.wav`, `~/.bash_history`, `/var/log/auth.log`,
   `/var/log/syslog`, `journalctl -o export` for the window.
4. Image the disk (`dd` to a clean device) before any clean-up — the forensic
   value drops the moment anyone starts "fixing".
5. Scan the physical environment for the second device (the Wi-Fi relay). It
   is probably still in the room, powered on, within 10 m of where the laptop
   was.
6. Rebuild the host from a known-good image. Do not try to clean it in place.

## 6. Non-recommendations (what *not* to spend on)

- A SOAR / SIEM "AI threat hunter" for an SMB with no logs to feed it. Fix
  the logging and the identity hygiene first.
- A gold-plated endpoint vendor on top of a flat network with no VLANs.
  The intruder would come straight back in next door.
- "AI blocker" marketing products. The thing you need to block is the
  *behaviour* (unknown MAC, cred reuse, process anomaly) — not the actor's
  tool choice.

## 7. Finding → control cross-reference

| Finding | §1 (immediate) | §2 (short-term) | §3 (medium-term) |
|---|---|---|---|
| F1 cred in history | 1.1 | — | 3.3 |
| F2 HID auto-accept | 1.2 | — | — |
| F3 LAN C2 undetected | 1.3 | 2.1 | 3.1 |
| F4 sudo w/ reusable pw | — | 2.3 | 3.3 |
| F5 redis exposed | — | 2.2 | 3.1 |
| F6 PwnKit unpatched | — | 2.2 | — |
| F7 no behavioural EDR | 1.3 | — | 3.2 |
| F8 unknown MAC on Wi-Fi | — | 2.1 | 3.1 |
