# Nmap Scan Report — scanme.nmap.org

**By:** [Your Name]
**Date:** September 12, 2026
**Tools:** Kali Linux, Nmap 7.94SVN
**Target:** scanme.nmap.org (45.33.32.156) — this is a host that the Nmap team put online on purpose so people can practice scanning it legally.

## What I did

I ran three Nmap scans against scanme.nmap.org to practice basic recon:

1. `nmap -sV scanme.nmap.org` — to see what ports are open and what service/version is running on each
2. `nmap -A scanme.nmap.org` — a more detailed scan (OS guess, scripts, etc.)
3. `nmap --script vuln scanme.nmap.org` — to run Nmap's built-in vulnerability-checking scripts

I saved the raw output of each scan in the `scans/` folder so anyone can see exactly what came back on my machine, not just my summary of it.

## What I found

### Open ports

| Port | Service | Version |
|---|---|---|
| 22/tcp | SSH | OpenSSH 6.6.1p1 (Ubuntu) |
| 80/tcp | HTTP | Apache 2.4.7 (Ubuntu) |
| 9929/tcp | nping-echo | Nping echo |
| 31337/tcp | tcpwrapped / "Elite" | — |

### Scan 1 — `-sV`

This just confirmed the 4 open ports above and gave me the service versions. Nothing more than that, it's the "quick overview" scan.

### Scan 2 — `-A`

This scan gave more detail on port 22 and 80:
- On port 22, it listed the SSH host keys (DSA, RSA, ECDSA, ED25519).
- On port 80, it picked up the page title ("Go ahead and ScanMe!") and confirmed it's running Apache 2.4.7.

Both of these versions (OpenSSH 6.6.1p1 and Apache 2.4.7) are old releases. I didn't confirm any specific vulnerability tied to the version numbers myself — that would need looking them up against a CVE database, which I haven't done yet. I'm just noting that they're old.

### Scan 3 — `--script vuln`

This is where the NSE (Nmap Scripting Engine) vulnerability scripts ran. Here's what came back on port 80:

- `http-csrf` flagged two search forms (`nst-head-search` and `nst-foot-search`) as **possible** CSRF issues. It says "possible" — it didn't confirm an actual exploit, just that the forms don't obviously have CSRF protection.
- `http-stored-xss` and `http-dombased-xss` both came back clean — no stored or DOM-based XSS found.
- `http-enum` found that `/images/` has directory listing turned on, meaning you can browse the folder contents directly in a browser.

No vulnerability scripts fired on port 22, 9929, or 31337.

## Notes / things I learned

- Just because an NSE script flags something as "possible" doesn't mean it's an actual vulnerability. I need to manually check things like the CSRF finding before calling it a real issue — the tool is a starting point, not a final answer.
- Port 31337 stood out to me because it's a well-known "hacker" port historically (Back Orifice, etc.). On this host it's expected because scanme.nmap.org is set up this way on purpose, but if I saw it open on a random target I'd want to look into it more.
- I didn't do anything past scanning — no exploitation, no login attempts, nothing beyond what these three commands returned.

## Recommendations / Next Steps

Based only on what these scans actually found, if this were a real system I was responsible for, I'd suggest:

1. **Update Apache and OpenSSH** to current versions, since both are old releases.
2. **Turn off directory listing** on `/images/` (in Apache this is done by removing `Indexes` from the `Options` directive, e.g. `Options -Indexes`).
3. **Add CSRF tokens** to the two search forms, or confirm with whatever framework is running the site that CSRF protection is already handled.

## Conclusion

This exercise demonstrated a basic reconnaissance and enumeration workflow using Nmap, from service discovery and version detection to automated NSE-based vulnerability checks. It also demonstrated the importance of interpreting automated scan results carefully and distinguishing potential findings from confirmed vulnerabilities.

## Raw output

See the `scans/` folder:
- `01_service_version_scan.txt`
- `02_aggressive_scan.txt`
- `03_vuln_script_scan.txt`
