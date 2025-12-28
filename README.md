# Tonnet Directory

Community-maintained directory of Tonnet relay nodes for anonymous TON access.

## Usage

Tonnet clients automatically fetch this list to discover available relays:

```
https://raw.githubusercontent.com/TONresistor/tonnet-directory/main/relays.json
```

## Relay Format

```json
{
  "version": 1,
  "updated": "YYYY-MM-DD",
  "relays": [
    {
      "name": "my-relay",
      "pubkey": "<64-char hex ADNL public key>",
      "address": "<ip>:<port>",
      "roles": ["entry", "middle", "exit"],
      "operator": "anonymous"
    }
  ]
}
```

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Human-readable relay name |
| `pubkey` | Yes | 64-character hex ADNL public key |
| `address` | Yes | IP:port for ADNL connection |
| `roles` | Yes | Array: `entry`, `middle`, `exit` |
| `operator` | No | Operator name or "anonymous" |

### Roles

- **entry**: Can be first hop (sees client IP, not destination)
- **middle**: Can be middle hop (sees neither)
- **exit**: Can be last hop (sees destination, not client IP)

## Add Your Relay

1. Run `tonnet-relay info` to get your pubkey
2. Fork this repo
3. Add your relay to `relays.json`
4. Submit a Pull Request

## Requirements

- Relay must be publicly accessible
- Stable uptime expected
- No logging of circuit data
