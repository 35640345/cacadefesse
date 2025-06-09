# 📈 Revenues Digital — Real-Time Revenue Dashboard

[![Live Site](https://img.shields.io/badge/Live-Site-14b8a6?logo=google-chrome&logoColor=white)](https://revenues.digital)
[![License](https://img.shields.io/badge/License-MIT-1e293b)](#license)

A real time revenue tracking platform for one of my projects, built on an AWS EC2 free-tier instance. The dashboard streams live sales data over WebSockets, renders rich interactive charts, and lets team-mates jump into a peer-to-peer voice room — all inside the browser.

---

## ✨ Feature Highlights

| Category | Details |
| :------- | :------ |
| **Real-Time Data** | Live sales & visitor metrics delivered via **Socket.IO WebSockets** (`wss://ws.revenues.digital:2087`) with automatic re-connect and exponential back-off. |
| **Interactive Charts** | Three responsive **Chart.js** visualizations (Revenue 30 days, Visitors 30 days, Visitors 24 h) that update in place without page reload. |
| **Voice Collaboration** | **PeerJS + WebRTC** voice channel (`https://webrtc.revenues.digital:2096`) with STUN fallback, mute/unmute, speaking indicators, and peer auto-discovery. |
| **Authentication** | Lightweight username + password (`test`) flow with session logout. |
| **Animations** | Smooth counters powered by **CountUp.js** for projected revenue. |
| **Design System** | Dark glass-morphic UI, **Inter** font, teal accents `#14b8a6`, gradient background, fully responsive. |
| **Deployment** | Automated SSL via **Certbot** on Ubuntu 22.04, running on an **AWS t2.micro** (free-tier) instance behind Nginx reverse proxy. |

---

---


Browse to **[https://localhost](https://localhost)** and log in with any **username** and the password **test**.

---

## 🖼️ Project Screenshots


### 🔑 Login Screen

To log-in, you can use:

**Username:** Visitor
**Password:** Test
![Login page](https://i.ibb.co/CpSGGyt1/image.png)

> Minimal dark-theme form. Any username + the password **test** grants access for demo purposes.


---

### 🔒 SSL Certificates (Certbot)

![Certbot issuing certificates](https://i.ibb.co/PZzDTCKc/image.png)

> Let’s Encrypt certificates provisioned automatically with **Certbot** on Ubuntu 22.04.

---

### 🗣️ PeerJS Server Console

![PeerServer](https://i.ibb.co/0pvpndvS/image.png)

> Self-hosted **PeerJS** signalling server running on the same EC2 instance (`https://webrtc.revenues.digital:2096`).

---

## 📊 Charts in Action

The dashboard integrates **Chart.js 4** exactly as documented:

* [Getting Started](https://www.chartjs.org/docs/latest/getting-started/usage.html)
* [Dynamic Updates](https://www.chartjs.org/docs/latest/developers/updates.html)

Real-time packets (`revenue_update`, `connection_update`, `stats`) patch the datasets and call `chart.update('none')` for buttery-smooth transitions.

---

## 🎙️ Voice Chat Details

* **PeerJS 1.5.2**: high-level WebRTC wrapper.
* **Signalling**: hosted at `https://webrtc.revenues.digital:2096`.
* **STUN**: `stun:stun.l.google.com:19302`, `stun:stun1.l.google.com:19302`.
* **Auto discovery**: `GET /peerjs/peers` then `peer.call(...)`.
* **Reconnection**: capped exponential back-off (`maxReconnectAttempts = 5`).

---

## ☁️ Infrastructure Choice

| Hosting Option         | Pros                                                                                                                                    | Cons                                                                            |
| :--------------------- | :-------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------ |
| **AWS EC2 (t2.micro)** | • Full customization / root access<br>• Real-time services, custom runtimes, any database<br>• Separate **frontend + backend** possible | • Requires setup, monitoring, Linux knowledge<br>• Higher learning curve        |
| **Wix.com**            | • Rapid site building, maintenance-free<br>• Built-in global CDN & 24/7 support                                                         | • **Cannot host custom back-end**<br>• Limited personalization beyond templates |

> **Verdict:** With WebSocket, PeerJS, custom ports, and automated SSL on the requirements list, **Amazon EC2** was the clear winner despite its steeper learning curve.

---

## 📚 References

* **Chart.js** — [https://www.chartjs.org/docs/latest/](https://www.chartjs.org/docs/latest/)
* **Socket.IO** — [https://socket.io/](https://socket.io/)
* **PeerJS** — [https://github.com/peers/peerjs](https://github.com/peers/peerjs)
* **CountUp.js** — [https://github.com/inorganik/CountUp.js](https://github.com/inorganik/CountUp.js)
* **Certbot** — [https://certbot.eff.org/](https://certbot.eff.org/)

---

## 🤝 Contributing

1. Fork this repo
2. Create a feature branch: `git checkout -b feat/amazing-thing`
3. Commit: `git commit -m "feat: amazing thing"`
4. Push: `git push origin feat/amazing-thing`
5. Open a Pull Request

We welcome code, docs, and design contributions!

---


```
```
