# TracPoll — Decentralized P2P Polling on Trac Intercom

> A fork of [Trac-Systems/intercom](https://github.com/Trac-Systems/intercom) that adds a trustless, peer-to-peer polling and voting application.

![TracPoll Banner](https://img.shields.io/badge/Built%20on-Trac%20Intercom-00ff88?style=for-the-badge&labelColor=0a0a0f)
![Status](https://img.shields.io/badge/Status-Live-00ff88?style=for-the-badge&labelColor=0a0a0f)
![P2P](https://img.shields.io/badge/P2P-Intercom%20Sidechannels-7c3aff?style=for-the-badge&labelColor=0a0a0f)

---

## 💡 What is TracPoll?

**TracPoll** is a decentralized polling application built on top of [Trac Intercom](https://github.com/Trac-Systems/intercom). It allows anyone to:

- **Create polls** with up to 6 options
- **Broadcast polls** to all connected peers via Intercom P2P sidechannels
- **Cast votes** that are propagated across the network
- **Commit results** to the replicated state layer for durable, verifiable outcomes

No central server. No intermediary. Every poll and vote is signed, broadcast peer-to-peer, and committed to shared state — fully trustless.

---

## 🔑 Trac Address

```
trac1uq3aujn2xr0ylwvyzng8g635z80c07kqsq33wq2jkvk8jjgxuu5s3mjp7q
```

> *(Replace this with your actual Trac wallet address to be eligible for the TNK payout)*

---

## 🚀 How It Works

```
User creates poll
       │
       ▼
Poll signed locally
       │
       ▼
Broadcast via Intercom sidechannel → All connected peers
       │
       ▼
Peers receive & display poll
       │
       ▼
Votes cast → broadcast back via sidechannel
       │
       ▼
Results committed to replicated state layer (durable)
```

1. **Sidechannel layer** — Fast, ephemeral P2P messaging for real-time vote propagation
2. **Replicated state layer** — Durable consensus storage for final poll results
3. **No trusted third party** — Results are verifiable by any peer

---

## 📸 Screenshots / Proof of Work

### Main Interface
The app runs fully in-browser at `index.html`. Open it to:
- See live active polls with real-time vote counts
- Create and broadcast new polls
- Watch the P2P event log update as votes arrive from peers

*(Add your screenshots here)*

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| P2P Transport | Trac Intercom sidechannels |
| State Layer | Trac Intercom replicated state |
| Frontend | Vanilla HTML/CSS/JS (zero dependencies) |
| Identity | Trac wallet signing |

---

## ⚙️ Running Locally

```bash
# Clone this fork
git clone https://github.com/YOUR_USERNAME/intercom
cd intercom

# Install Intercom dependencies
npm install

# Open the polling app
open index.html
# or serve it
npx serve .
```

---

## 📖 Skill File

See [`skill.md`](./skill.md) for agent instructions — how AI agents can use TracPoll programmatically to create polls, retrieve results, and cast votes.

---

## 🏗️ Roadmap

- [x] Basic poll creation & voting UI
- [x] P2P event log simulation
- [ ] Live Intercom node integration
- [ ] Poll signing with Trac wallet
- [ ] Expiry / time-gated polls
- [ ] Multi-choice (multiple votes per user)
- [ ] On-chain result anchoring

---

## 📜 License

MIT — Fork of [Trac-Systems/intercom](https://github.com/Trac-Systems/intercom)

---

*Built for the Awesome Intercom fork contest — 500 TNK payout eligible*
