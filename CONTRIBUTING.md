# Contributing to akash-miner-blacklist

Thanks for helping keep Akash Network provider infrastructure clean.

This document describes how to submit a tenant address you've observed exhibiting miner behaviour, and the evidence standards required for that submission to land in the list.

## The core principle

The credibility of this list depends entirely on its evidence standards. **Weak evidence poisons the whole list.** One badly-attributed address discredits the other 100 well-attributed ones.

So: don't submit an address unless you can back it up.

## How to submit

1. Open a new issue using the [report-miner template](.github/ISSUE_TEMPLATE/report-miner.md)
2. Fill in every field the template asks for
3. Wait for maintainer review — usually within 72 hours
4. Maintainer either accepts (and lands a PR adding the address) or asks clarifying questions

Do not open a PR directly against `blacklist.txt`. Submit via issue first so evidence is reviewed before the data lands.

## What counts as valid evidence

You need at least one of the following, and the more the better:

### Primary evidence (any one is sufficient)

- **Automated detection output** — log line from `evict-miners.sh`, `check-cpu-miners.sh`, or your own equivalent that caught the tenant. Include the timestamp and the specific pattern that matched.
- **Process listing on the worker node** — `ps aux` or equivalent showing the miner process with sustained high CPU. Ideally with the `AKASH_OWNER` environment variable extracted from `/proc/PID/environ` linking the process to the tenant address.
- **Container image name** — the pod's container image contains an unambiguous miner tag (e.g. `xmrig`, `nbminer`, `t-rex`, `phoenixminer`, `lolminer`).
- **Named workload** — the pod name itself contains a miner keyword (`miner`, `xmrig`, `monero`, and similar).

### Supporting evidence (strengthens a submission)

- **Bidding pattern** — chain-observable behaviour that's characteristic of scanner-bots or repeated miner deployment attempts (high frequency, never winning, always requesting the same resource shape)
- **Cross-provider observation** — if the same tenant address has been reported by multiple operators
- **Historical activity** — the address has a public deployment history that shows repeated miner activity across time

### Insufficient evidence (do NOT submit based on)

- "This tenant's deployment felt suspicious"
- "Someone in Discord said..."
- "This address was blacklisted by another provider" (without the underlying evidence)
- Speculation about tenant identity based on wallet activity patterns alone
- A single deployment that looked mining-adjacent but where you never actually observed miner behaviour

## What information the issue should include

The report-miner template asks for these fields. Fill them in:

- **Tenant address** — full `akash1...` address
- **Category** — `cpu-miner`, `gpu-miner`, `scanner-bot`, or `other` (see README for definitions)
- **Observation window** — approximate month(s), e.g. `2026-06` or `2026-05-to-2026-07`
- **Evidence phrase** — one line summarizing what you observed. Examples:
  - `xmrig image, cron detection`
  - `python3 main.py sustained 400% CPU on gpu node`
  - `high-freq scanner, 30+ bids/day, never won`
- **Detection detail** — the actual log line, process listing, or image name. Redact any of your own infrastructure detail (internal IPs, hostnames, wallet addresses) before pasting.
- **Provider region** — your provider's region (e.g. `au-syd`, `us-east`, `eu-west`) — helps correlate submissions across geography

## What NOT to include in your issue

Please redact before submitting:

- Your own provider wallet address
- Internal IPs, subnets, hostnames
- PVC paths, keyring paths, TLS keys
- Anything from your `values.yaml` or provider secrets

The tenant address is the only address that should appear.

## Review criteria

The maintainer will accept a submission if:

- The tenant address is well-formed (`akash1` prefix, 38 characters after)
- At least one form of primary evidence is included
- The category label is reasonable given the evidence
- The observation window is stated
- No redaction issues in the submitted detail

The maintainer will ask for clarification (not reject) if:

- Evidence is present but ambiguous
- Category is unclear from the observation
- The observation window seems too vague

The maintainer will decline a submission if:

- Evidence is speculation or hearsay
- The tenant address is already in the list (in which case, just add your observation as a comment on the existing entry's PR)
- The submission includes information that shouldn't be public

## Amending an existing entry

If you have more evidence on an address that's already in the list — especially an `unknown` category entry with rich new evidence — file an issue titled `[amend] akash1...` and describe the amendment. Maintainer will PR the update.

## Un-blacklisting

If you believe an address has been blacklisted in error:

1. Open an issue titled `[dispute] akash1...`
2. Explain why you believe the classification is wrong
3. Include any counter-evidence (legitimate use case, tenant statement, etc.)

Maintainer will review. Removals require the same evidence bar as additions — if the original evidence was strong, the counter-evidence needs to be equally strong.

## Attribution

Contributors are credited in the entry itself via a `Reported-by:` comment adjacent to the address. If you'd rather not be publicly attributed, mention that in the issue and the entry will land without a reporter tag.

## Questions

Open an issue titled `[question] ...` for anything not covered here. Maintainer responds within roughly 72 hours.
