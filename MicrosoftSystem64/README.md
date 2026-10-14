# MicrosoftSystem64

Indicators from Island Security Research's investigation into the
**MicrosoftSystem64** malware cluster, a supply chain campaign that delivers a
cross-platform remote access trojan (RAT) through malicious npm dependency
chains wired into credible-looking public GitHub repositories.

Companion to [How Attackers Built Software an AI Would Recommend](https://www.island.io/blog/how-attackers-built-software-an-ai-would-recommend).


The name MicrosoftSystem64 comes from the persistence identity used by the
malware across Windows, macOS, and Linux. It is unrelated to Microsoft
Corporation.

## Datasets

### [`microsoftsystem64-iocs-2026-09.csv`](microsoftsystem64-iocs-2026-09.csv)

Technical indicators for the delivery chain and payload.

| Column | Description |
| --- | --- |
| `type` | Indicator category (npm package, c2, persistence, host artifact, sha-256, and `historical` variants) |
| `indicator` | The indicator value |
| `context` | How the indicator relates to the campaign |

The chain combines transitive dependencies with SemVer-compatible updates: a
top-level carrier package pulls a weaponized transitive carrier, which pulls the
payload package that executes through `postinstall`. The top-level carrier name
rotates over time (`ioredis-os` -> `ioredis-xyz` -> `oscar-redis` ->
`oracle-redis`) while the repository stays in place, so **a package-name IOC
goes stale quickly**. Prefer behavior-based detection and track the repositories
as the durable distribution asset.

### [`microsoftsystem64-repositories-2026-09.txt`](microsoftsystem64-repositories-2026-09.txt)

88 public GitHub repositories linked to the campaign as of 2026-09-20, one
`owner/repo` identifier per line. Prefix with `https://github.com/` to resolve.
The final two entries were recovered without an owner and appear as bare repo
names.

These are not empty repositories built around a malicious dependency. Many
contain real, working software -- copied or repurposed open source projects with
imported or synthetically generated commit history and automated fork networks
-- which is what makes the malicious dependency plausible. Treat a listed
repository as campaign-linked even if its current dependency no longer matches a
known carrier package name.

## Handling

Indicators are a dated snapshot, not a substitute for behavior-based detection.
This campaign rotates its carrier packages quickly while preserving the
surrounding repositories.

A repository or hostname referencing a brand, platform, or vendor does not imply
that the named party was compromised or involved. The repositories impersonate
integrations for legitimate services.

The information in this repository is provided as is for defensive security
research purposes.
