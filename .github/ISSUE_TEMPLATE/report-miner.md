---
name: Report a miner
about: Report a tenant address that has exhibited miner behaviour on your provider
title: '[report] akash1...'
labels: 'report-miner'
assignees: ''
---

<!--
Thanks for contributing to akash-miner-blacklist.

Before submitting, please read CONTRIBUTING.md for evidence standards.
Weak submissions dilute the credibility of the whole list.

Fill in every field below. Redact your own infrastructure detail before pasting logs.
-->

## Tenant address

<!-- Full akash1... address (38 characters after akash1). -->



## Category

<!-- Pick one: cpu-miner, gpu-miner, scanner-bot, other -->



## Observation window

<!-- Approximate month(s) - e.g. 2026-06, or 2026-05-to-2026-07 -->



## Evidence phrase

<!--
One line summarizing what you observed. Examples:
  xmrig image, cron detection
  python3 main.py sustained 400% CPU on GPU node
  high-freq scanner, 30+ bids/day, never won
-->



## Detection detail

<!--
The actual evidence. Log line, process listing, container image name,
or bidding pattern observation.

REDACT before pasting:
- Your provider wallet address
- Internal IPs, subnets, hostnames
- PVC paths, keyring paths, TLS keys
- Anything from values.yaml or provider secrets

The tenant address is the only address that should appear.
-->

    paste your evidence here as an indented code block

## Provider region

<!-- Your provider region - e.g. au-syd, us-east, eu-west, ap-southeast -->



## Attribution preference

<!--
Choose one:
  [ ] Credit me in the entry (Reported-by: my-github-handle)
  [ ] Add anonymously (no reporter tag)
-->

- [ ] Credit me
- [ ] Anonymous

## Confirmation

<!-- Confirm each of these by replacing [ ] with [x] -->

- [ ] I have personally observed this address exhibit miner behaviour
- [ ] The evidence above is from my own observation, not hearsay
- [ ] I have redacted my own infrastructure detail from the pasted evidence
- [ ] I have read CONTRIBUTING.md
