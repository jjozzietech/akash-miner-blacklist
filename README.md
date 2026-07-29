# // akash-miner-blacklist

Community-observed tenant addresses that have exhibited miner behaviour on Akash Network providers.

Sanitized from a production `au-syd` provider's live blacklist. Real observations, honest provenance. Contribution flow for other operators to add addresses they've caught.

## // why this exists

Miner tenants burn compute, evict legitimate deployments, drain provider wallets via gas, and waste capacity for no ecosystem benefit. Every provider defends against them independently, catches roughly the same addresses, and rediscovers the same patterns.

This repo is the shared list — an abuse-intelligence resource that operators can pull from and contribute to. Nothing here replaces per-provider defense (see the [companion scripts](https://github.com/jjozzietech/akash-provider-ops-public/tree/main/anti-mining) for that). This is the *data layer* the scripts consume.

## // how the initial data was collected

The 37 entries in `blacklist.txt` came from one production provider's live defense system between roughly December 2025 and July 2026. Two automated detection scripts fed the list:

- **Pattern-based detection** — pod names and container images matched against known miner keywords (`xmrig`, `miner`, `monero`, and similar)
- **Behavioural detection** — sustained CPU load on worker nodes, with tenant identified via `AKASH_OWNER` extracted from `/proc/PID/environ`

Both scripts auto-appended matching tenants to the blacklist. See [`akash-provider-ops-public/anti-mining/`](https://github.com/jjozzietech/akash-provider-ops-public/tree/main/anti-mining) for the actual scripts.

## // honest disclosure — provenance quality

The eviction log wasn't preserved across `/var/log/miner-eviction.log` rotations. As a result:

- **3 entries** have rich per-address evidence (the July 2026 scanner-bot batch, documented in operational session notes)
- **34 entries** are marked `unknown` — genuinely, we don't have preserved per-address evidence beyond "our production defense caught them"

This is deliberately honest rather than reverse-engineered. The 34 unknowns are real observations that our production scripts acted on; the evidence trail just wasn't retained. Community submissions going forward will land with proper per-entry provenance.

If you're deciding whether to trust the 34 unknowns: they were caught by the same scripts and thresholds we've published openly. If you run those scripts, you'd catch the same addresses. That's the whole argument for the data.

## // what's in here

| File | What it is |
|---|---|
| [`blacklist.txt`](./blacklist.txt) | 37 tenant addresses with category, observation window, evidence |
| [`CONTRIBUTING.md`](./CONTRIBUTING.md) | Evidence standards for community submissions |
| [`.github/ISSUE_TEMPLATE/report-miner.md`](./.github/ISSUE_TEMPLATE/report-miner.md) | Structured template for reporting a new miner |

## // format

Each line follows this shape:

    akash1<address>    <category>    <window>    <evidence phrase>

Fixed-position columns separated by whitespace. Comments start with `#`. Blank lines allowed.

Categories in use:

- `scanner-bot` — high-frequency bidders that never win, likely scraping bid history or benchmarking
- `unknown` — auto-detected by production scripts, per-entry evidence not preserved
- `cpu-miner`, `gpu-miner` — not currently populated; reserved for community submissions with confirmed evidence
- `other` — reserved for edge cases

## // how to use

Two integration paths:

**Path 1 — Pull as a reference list.**
```bash
curl -sSL https://raw.githubusercontent.com/jjozzietech/akash-miner-blacklist/main/blacklist.txt \
  | awk '/^akash1/ {print $1}' > local-blacklist.txt
```

Review. Merge with your own local blacklist (whatever format your bid script expects). Don't just concatenate — inspect entries first, especially the `unknown` category. False positives on someone else's list can cost you legitimate tenants.

**Path 2 — Contribute back.**
Run the [companion scripts](https://github.com/jjozzietech/akash-provider-ops-public/tree/main/anti-mining) on your own provider. When they catch something, [file an issue](https://github.com/jjozzietech/akash-miner-blacklist/issues/new?template=report-miner.md) with the evidence.

## // how to contribute

See [CONTRIBUTING.md](./CONTRIBUTING.md) for evidence standards.

Short version: file an issue using the report-miner template. Include the tenant address, the evidence pattern (image name, process name, load profile, bidding cadence), and rough observation window. Maintainer accepts via PR after review.

Do not:
- Submit addresses you haven't personally observed
- Submit addresses based on "someone in Discord said..."
- Submit addresses tied to a single deployment that "seemed suspicious"
- Speculate on tenant identity

Do:
- Submit addresses you've caught via automated detection
- Include the actual detection pattern (log line, process listing, image name)
- Note your provider region if that adds context

## // what's NOT in this list

- Tenant addresses reported by other operators without evidence
- Anything the maintainer hasn't personally observed OR verified via a submitted issue
- Speculation, rumours, Discord chat
- Wallet addresses of any kind that aren't tenant addresses (provider addresses, cold storage, treasury — none belong here)

The credibility of a blacklist depends entirely on its evidence standards. Weak evidence poisons the whole list.

## // false positive risk

Every entry on this list is a claim, not a proof. Operators using this data should:

1. Review each entry before adding to their local blacklist
2. Consider whether their tenant mix might legitimately match the observed patterns (some workloads look mining-adjacent — video encoding, scientific compute, ML training)
3. Treat `unknown`-category entries with more scrutiny than richly-documented ones
4. Have a mechanism to un-blacklist if a legitimate tenant is affected

The publishing operator is not responsible for downstream decisions. This is data, not policy.

## // the companion scripts

Two scripts on the operator's provider generate entries here:

- [`evict-miners.sh`](https://github.com/jjozzietech/akash-provider-ops-public/tree/main/anti-mining) — pattern-based detection
- [`check-cpu-miners.sh`](https://github.com/jjozzietech/akash-provider-ops-public/tree/main/anti-mining) — behavioural detection

Both auto-append matching tenants to the local bid script's blacklist and fire `helm upgrade` to inject the new list into the running provider. See that repo for the full architecture.

## // the operator

Maintained by [jjozzietech](https://github.com/jjozzietech), running a production Akash provider in `au-syd`.

- Site — [jjozzietech.com.au](https://jjozzietech.com.au)
- Companion ops repo — [akash-provider-ops-public](https://github.com/jjozzietech/akash-provider-ops-public)
- Provider address — `akash1sev...wd4e` ([verifiable via Akash Console](https://akash.cp0x.com/providers/akash1sevd2ymtty3dpq9ycxgkhuzzk4fe6mchqdwd4e))

## // license

MIT — see [LICENSE](./LICENSE). The blacklist is data; MIT means you can use it however you want. Attribution appreciated but not required.
