````markdown
# 📈 Revenues Digital — Real-Time Revenue Dashboard

[![Live Site](https://img.shields.io/badge/Live-Site-14b8a6?logo=google-chrome&logoColor=white)](https://revenues.digital)
[![License](https://img.shields.io/badge/License-MIT-1e293b)](#license)

A full-stack, **real-time revenue monitoring platform** with collaborative voice chat, built from the ground up on an Amazon EC2 free-tier instance. The dashboard streams live sales data over WebSockets, renders rich interactive charts, and lets team-mates jump into a peer-to-peer voice room — all inside the browser.

---

## ✨ Feature Highlights

| Category | Details |
| -------- | ------- |
| **Real-Time Data** | Live sales & visitor metrics delivered via **Socket.IO WebSockets** (`wss://ws.revenues.digital:2087`) with automatic re-connect and exponential back-off. |
| **Interactive Charts** | Three responsive **Chart.js** visualizations (Revenue 30 days, Visitors 30 days, Visitors 24 hrs) that update in place without page reload. |
| **Voice Collaboration** | **PeerJS + WebRTC** voice channel (`https://webrtc.revenues.digital:2096`) with STUN fallback, mute/unmute, speaking indicators, and peer auto-discovery. |
| **Authentication** | Lightweight username + password (`test`) flow with session logout. |
| **Animations** | Smooth counters powered by **CountUp.js** for projected revenue. |
| **Design System** | Dark glass-morphic UI, **Inter** font, teal accents `#14b8a6`, gradient background, fully responsive. |
| **Deployment** | One-click SSL via **Certbot** on Ubuntu 22.04, running on an **AWS t2.micro (free-tier)** instance behind Nginx reverse proxy. |

---

## 🏗️ Architecture Overview

```text
┌──────────────┐      wss://ws.revenues.digital
│  Socket.IO   │ ───────────────────────────────▶  Client
│  Server      │         (live metrics)         ┌──────────────┐
└──────────────┘                                 │ ChartManager │
                                                 └──────────────┘
      ▲                                               ▲
      │ HTTPS                                         │ WebRTC
      │                                               ▼
┌──────────────┐  signaling  https://webrtc.revenues.digital  ┌──────────────┐
│  PeerServer  │ ◀─────────────────────────────────────────── │ PeerService  │
└──────────────┘                                              └──────────────┘
````

* **Modular JS classes** handle each concern (`AuthManager`, `SocketService`, `PeerService`, `ChartManager`, `DashboardManager`, `VoiceChatManager`).
* **Nginx** terminates TLS (Let’s Encrypt) and proxies WebSocket and PeerJS traffic to Node services.
* **No external DB** — sales data is streamed in real-time by the origin business platform.

---

## 🚀 Getting Started

> **Prerequisites:** Node 18 +, npm, and a modern browser with mic support.

1. **Clone & install**

   ```bash
   git clone https://github.com/your-org/revenues-digital.git
   cd revenues-digital
   npm install
   ```

2. **Environment**

   ```bash
   cp .env.example .env
   # tweak ports / domains as needed
   ```

3. **Run all services**

   ```bash
   # WebSocket API
   npm run start:ws
   # PeerJS Signalling
   npm run start:peer
   # Static Front-End (Vite / Live-Server / Nginx)
   npm run dev
   ```

4. **Browse**

   ```text
   https://localhost
   ```

   Log in with any **username** and the password **test**.

---

## 🖼️ Screenshots

| Login Page                                    | Dashboard                                           | Voice Peers                                        |
| --------------------------------------------- | --------------------------------------------------- | -------------------------------------------------- |
| ![Login](https://i.ibb.co/CpSGGyt1/image.png) | ![Certbot SSL](https://i.ibb.co/PZzDTCKc/image.png) | ![PeerServer](https://i.ibb.co/0pvpndvS/image.png) |

*(All screenshots taken on the production EC2 host.)*

---

## 📊 Charts in Action

The dashboard integrates **Chart.js 4** exactly as described in the official docs:

* [Getting Started](https://www.chartjs.org/docs/latest/getting-started/usage.html)
* [Dynamic Updates](https://www.chartjs.org/docs/latest/developers/updates.html)

Each update packet (`revenue_update`, `connection_update`, `stats`) mutates the underlying dataset, followed by `chart.update('none')` for 60 fps-smooth transitions.

---

## 🎙️ Voice Chat Details

* **PeerJS** (`@1.5.2`) provides a high-level wrapper around WebRTC.
* Signalling server runs on the same EC2 box (`https://webrtc.revenues.digital:2096`).
* STUN fallback: `stun:stun.l.google.com:19302`, `stun:stun1.l.google.com:19302`.
* Automatic peer discovery via `GET /peerjs/peers`, then `peer.call(...)`.
* Robust reconnection logic with capped exponential backoff (`maxReconnectAttempts = 5`).

---

## ☁️ Infrastructure Choice

| Hosting Option         | Pros                                                                                                                               | Cons                                                                          |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **AWS EC2 (t2.micro)** | • Full customization / root access<br>• Real-time services, custom runtimes, any DB<br>• Separate **frontend + backend** supported | • Requires setup, monitoring, Linux skills<br>• Higher learning curve         |
| **Wix.com**            | • Rapid site building, no maintenance<br>• Built-in global CDN & 24/7 support                                                      | • **No custom backend** hosting<br>• Limited personalization beyond templates |

> **Verdict:** The project’s need for WebSocket, PeerJS, custom ports, and SSL automation made **Amazon EC2** the only viable choice despite its steeper learning curve.

---

## 🔒 Security Notes

* TLS everywhere — issued via [Certbot](https://certbot.eff.org/) on Ubuntu with auto-renew.
* WebSocket traffic upgraded through Nginx with `proxy_read_timeout 900s`.
* Microphone access gated by an **opt-in** browser prompt.

---

## 📚 Documentation & References

* **Chart.js** — [https://www.chartjs.org/docs/latest/](https://www.chartjs.org/docs/latest/)
* **Socket.IO 4.7** — [https://socket.io/](https://socket.io/)
* **PeerJS** — [https://github.com/peers/peerjs](https://github.com/peers/peerjs)
* **CountUp.js** — [https://github.com/inorganik/CountUp.js](https://github.com/inorganik/CountUp.js)
* **Let’s Encrypt / Certbot** — [https://certbot.eff.org/](https://certbot.eff.org/)

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch `git checkout -b feat/amazing-thing`
3. Commit your changes `git commit -m "feat: amazing thing"`
4. Push to GitHub `git push origin feat/amazing-thing`
5. Open a Pull Request

All contributions (code, docs, design) are welcome!

---

## 📝 License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for full text.

```

> **Need help or spot a bug?** Open an issue or start a discussion — we’re growing this project together!
```
