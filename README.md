# Nmap Recon Practice — scanme.nmap.org

This is one of my first hands-on networking/security projects. I used Nmap
on Kali Linux to scan scanme.nmap.org, which is a host the Nmap project set
up specifically for people to practice scanning against legally.

## Why scanme.nmap.org

Scanning a system without permission is illegal, so I picked a target that
is explicitly allowed to be scanned: https://nmap.org/book/testing.html

## What's in this repo

```
nmap-recon-scanme/
├── README.md
├── report/
│   └── scan-report.md      <- write-up of what I ran and what I found
└── scans/
    ├── 01_service_version_scan.txt
    ├── 02_aggressive_scan.txt
    └── 03_vuln_script_scan.txt
```

## What I ran

1. `nmap -sV scanme.nmap.org` — basic port + service version scan
2. `nmap -A scanme.nmap.org` — more detailed scan
3. `nmap --script vuln scanme.nmap.org` — Nmap's vulnerability-checking scripts

## Quick summary of results

- 4 open ports: 22 (SSH), 80 (HTTP), 9929 (nping-echo), 31337 (tcpwrapped)
- Web server is Apache 2.4.7, SSH is OpenSSH 6.6.1p1 — both older versions
- The vuln scan flagged two forms on the site as *possibly* vulnerable to CSRF (not confirmed, just flagged)
- No XSS was found
- Directory listing is turned on for `/images/`

Full details are in [report/scan-report.md](report/scan-report.md).
See screenshots/ for terminal and browser evidence from these scans.

**Skills practiced:** port scanning, service/version enumeration, reading NSE script output, distinguishing confirmed findings from possible/automated flags.

## What I'd like to try next

- Learning how to actually check if the CSRF flag is a real issue, not just trust the script
- Looking up the software versions against a CVE database
- Trying a scan on a different practice target (like TryHackMe or HackTheBox)

## About me

I'm a student learning the basics of networking and security. This repo is
me documenting my practice as I go, so feedback is welcome.
