# NovaCookies

Indicators from Island Security Research's investigation into NovaCookies, a
commercial adversary-in-the-middle phishing service that relays Microsoft 365
sign-ins to capture authenticated sessions.

Companion to [NovaCookies at scale: Inside the $320 Phishing Service Targeting
Hundreds of Organizations](https://www.island.io/blog/novacookies-at-scale-inside-the-320-phishing-service-targeting-hundreds-of-organizations).

## Dataset

[`novacookies-phishing-domains-2026-08.txt`](novacookies-phishing-domains-2026-08.txt) holds 755 domains, one per line.

Every entry is a registered domain assessed as part of the NovaCookies phishing
infrastructure: the hosts that served the campaign's counterfeit Microsoft 365
sign-in pages and relayed those sign-ins to capture the resulting sessions. These
are domains registered and operated for the campaign, not legitimate sites that
were compromised. Compromised third-party hosts are deliberately excluded, for the
reason given below.

Domains are **defanged**: replace `[.]` with `.` before use, for example

    abcfinasyoncialservices[.]vu   ->   abcfinasyoncialservices.vu

Entries are at the registered-domain level. Lure hosts appear on subdomains of
these, so the entry covers them.

The file is grouped into three confidence sections by `#` comment headers, which
standard blocklist tooling ignores. Read them before enforcing.

**`confirmed`** (736): carries the kit's own lure-path signature, renders one of its verification gates, or a recovered operator artifact ties the host to the service.

**`probable`** (8): matches the campaign's hostname convention and shows the kit's gate parameters, or was reached through the campaign's own redirection chain.

**`possible`** (11): matches the campaign's hostname convention and its registration profile, without a behavioral observation of the kit itself. Treat these as leads for monitoring rather than as verified malicious infrastructure.

## What is deliberately not here

Every candidate was checked against the registration profile of the confirmed
set, which is concentrated on a small number of registrars inside a known
registration window. Excluded rather than published:

- domains whose registrar or registration date fell outside that profile
- legitimate sites that were compromised and served campaign content, since
  blocking them at the domain level harms their owners
- shared hosting and cloud provider domains, where a lure occupied a subdomain
  the provider owns
- hosts whose apex reads as an unrelated real business, where a long
  campaign-style subdomain indicates the host was compromised rather than
  registered by the operators

## Handling

Indicators are a dated snapshot, not a substitute for behavior-based detection.
This campaign rotates infrastructure quickly.

A domain appearing here is not evidence that any company named or resembled in a
hostname was compromised. Hostnames in this campaign frequently mutate
recognizable company names, and the borrowed name is not reliably the targeted
organization.

The dataset excludes victim data, personalized lure URLs, operator notification
channels and identifiers, internal telemetry and detection logic.
