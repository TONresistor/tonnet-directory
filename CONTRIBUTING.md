# Contributing a Relay

This guide explains how to add your relay to the Tonnet directory.

## Prerequisites

1. A running `tonnet-relay` instance accessible from the internet
2. Ports open: `9001` (ADNL) and `9002/udp` (tunnel)
3. A GitHub account

## Step 1: Get Your Relay Info

```bash
tonnet-relay info
```

Output example:
```
Node Name:    my-relay
Public Key:   92651246183f098f7b0e943d6680a58b81b1ed14b9bc8b442126a6a653071997
ADNL Address: 80.78.27.15:9001
```

Save your **Public Key** and **ADNL Address** - you'll need them.

## Step 2: Verify Your Relay is Reachable

Before submitting, ensure your relay accepts connections:

```bash
# From another machine or ask someone to test
relay-test -target <your-pubkey>@<your-ip>:9001 -ping
```

Expected: `Ping successful`

## Step 3: Fork and Clone

```bash
# Fork via GitHub UI, then:
git clone https://github.com/<your-username>/tonnet-directory.git
cd tonnet-directory
```

## Step 4: Add Your Relay

Edit `relays.json` and add your entry to the `relays` array:

```json
{
  "name": "my-relay",
  "pubkey": "92651246183f098f7b0e943d6680a58b81b1ed14b9bc8b442126a6a653071997",
  "address": "80.78.27.15:9001",
  "roles": ["entry", "middle", "exit"],
  "operator": "anonymous"
}
```

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique name for your relay |
| `pubkey` | Yes | 64-char hex from `tonnet-relay info` |
| `address` | Yes | Public IP:port |
| `roles` | Yes | See roles below |
| `operator` | No | Your name or "anonymous" |

### Roles Explained

| Role | Description | Requirements |
|------|-------------|--------------|
| `entry` | First hop - sees client IP | Basic relay |
| `middle` | Middle hop - sees nothing | Basic relay |
| `exit` | Last hop - connects to TON sites | Requires `--exit` flag and `mainnet.json` |

Most relays should use `["entry", "middle"]`. Only add `"exit"` if you run with exit mode enabled.

## Step 5: Validate JSON

```bash
# Check JSON syntax
python3 -m json.tool relays.json > /dev/null && echo "Valid JSON"
```

## Step 6: Commit and Push

```bash
git add relays.json
git commit -m "Add <your-relay-name> relay"
git push origin main
```

## Step 7: Open a Pull Request

1. Go to https://github.com/TONresistor/tonnet-directory
2. Click "Pull requests" → "New pull request"
3. Select your fork and branch
4. Title: `Add <your-relay-name> relay`
5. Description: Include your relay's public address for verification

## Checklist Before Submitting

- [ ] Relay is running and accessible
- [ ] Ports 9001 and 9002/udp are open
- [ ] `pubkey` matches output of `tonnet-relay info`
- [ ] `address` is your public IP (not 127.0.0.1 or 0.0.0.0)
- [ ] JSON is valid (no trailing commas, proper quotes)
- [ ] Relay name is unique in the list

## Removing Your Relay

If you stop running your relay, please submit a PR to remove it from the list.

## Questions?

Open an issue at https://github.com/TONresistor/tonnet-directory/issues
