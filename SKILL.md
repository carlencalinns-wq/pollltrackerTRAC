# TracPoll Skill File

> Instructions for AI agents interacting with the TracPoll decentralized polling application via Trac Intercom.

---

## Overview

TracPoll is a P2P polling app on Trac Intercom. Agents can:
- Create new polls and broadcast them to the network
- Query existing polls and their current vote tallies
- Cast votes on behalf of a user (with user authorization)
- Subscribe to poll result updates via Intercom sidechannels

---

## Agent Capabilities

### 1. Create a Poll

**Trigger phrases:**
- "create a poll about X"
- "broadcast a vote for Y"
- "start a community poll"

**Required parameters:**
```json
{
  "action": "create_poll",
  "question": "string (required) — the poll question",
  "options": ["string", "string", "..."] ,
  "tag": "string (optional) — category e.g. GOVERNANCE, UX, TECH",
  "trac_address": "string — creator's Trac wallet address"
}
```

**Agent steps:**
1. Collect `question` and at least 2 `options` from user
2. Sign payload with user's Trac identity
3. Broadcast via `intercom.sidechannel.send({ type: 'poll_create', payload })`
4. Await acknowledgment from ≥1 peer
5. Confirm to user: poll ID and number of peers reached

---

### 2. List Active Polls

**Trigger phrases:**
- "show me active polls"
- "what polls are running"
- "list current votes"

**Agent steps:**
1. Query local replicated state: `intercom.state.get('tracpoll:polls')`
2. Return structured list: poll ID, question, options, vote counts, tag
3. Format as readable summary for user

---

### 3. Cast a Vote

**Trigger phrases:**
- "vote for X on poll Y"
- "I choose option B"
- "cast my vote"

**Required parameters:**
```json
{
  "action": "vote",
  "poll_id": "string — the poll ID (e.g. POLL-A1B2C3D4)",
  "option_index": "integer — 0-based index of chosen option",
  "trac_address": "string — voter's Trac wallet address"
}
```

**Agent steps:**
1. Verify poll exists in replicated state
2. Check user has not already voted (duplicate check by trac_address)
3. Sign vote payload
4. Broadcast: `intercom.sidechannel.send({ type: 'poll_vote', payload })`
5. Await state commit confirmation
6. Return updated vote tally to user

---

### 4. Get Poll Results

**Trigger phrases:**
- "results for poll X"
- "who is winning poll Y"
- "show vote counts"

**Agent steps:**
1. Fetch from state: `intercom.state.get('tracpoll:poll:' + poll_id)`
2. Calculate percentages per option
3. Return: winner (if decided), vote distribution, total voters

---

## Intercom Protocol Messages

| Message Type | Direction | Description |
|-------------|-----------|-------------|
| `poll_create` | broadcast | New poll announcement |
| `poll_vote` | broadcast | Vote cast event |
| `poll_sync` | request/response | Fetch all polls from a peer |
| `poll_result` | state commit | Final tally committed to replicated state |

---

## State Schema

```json
// Key: "tracpoll:polls"
{
  "polls": ["POLL-ID1", "POLL-ID2"]
}

// Key: "tracpoll:poll:{POLL_ID}"
{
  "id": "POLL-A1B2C3D4",
  "question": "Which chain next?",
  "options": ["Solana", "Ethereum", "TON"],
  "votes": [14, 9, 5],
  "voters": ["trac1abc...", "trac1def..."],
  "tag": "GOVERNANCE",
  "created_by": "trac1xyz...",
  "created_at": 1700000000000,
  "status": "active"
}
```

---

## Error Handling

| Error | Meaning | Agent response |
|-------|---------|----------------|
| `DUPLICATE_VOTE` | User already voted | Inform user, show current results |
| `POLL_NOT_FOUND` | Poll ID doesn't exist | Suggest listing active polls |
| `NO_PEERS` | No Intercom peers connected | Retry after 5s, notify user if persists |
| `INVALID_OPTION` | Option index out of range | Ask user to re-select |

---

## Example Agent Interaction

```
User: "Create a poll asking what language to use for the next Trac tool"

Agent:
1. Question: "What programming language should we use for the next Trac tool?"
2. Options: ["Rust", "Go", "TypeScript", "Python"]
3. Tag: "TECH"
4. Signs + broadcasts via Intercom sidechannel
5. Responds: "Poll POLL-X9Y8Z7W6 created and broadcast to 5 peers! Share the poll ID so others can vote."
```

---

## Notes for Agents

- Always confirm user intent before casting a vote (votes are immutable once committed to state)
- One Trac address = one vote per poll (enforced at state layer)
- Polls do not expire automatically in v0.1 — a future version will add TTL
- Agents should gracefully handle offline/no-peers scenarios with a clear user message
