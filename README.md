
# 📈 Revenues.Digital – Real-Time Revenue Dashboard

[![Live Site](https://img.shields.io/badge/Live%20Demo-Revenues.Digital-14b8a6?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0nMTAwJyBoZWlnaHQ9JzIwJy8+)](https://revenues.digital)

A **vanilla&nbsp;JavaScript** single-page application that streams sales data over WebSockets, renders interactive charts in real time, and lets teammates jump into a peer-to-peer voice room—all running on an Amazon EC2 *t2.micro* free-tier instance.

---

## ✨ Key Features

| Category | Highlights |
| -------- | ---------- |
| **Real-Time Data** | • Bi-directional **WebSocket** transport (Socket.IO)<br>• Infinite auto-reconnect with exponential back-off |
| **Visual Analytics** | • 30-day **Revenue** & **Visitors** line charts<br>• 24-hour **Visitors / Hour** bar chart<br>• Smooth value transitions with `Chart.update()` |
| **Collaboration** | • **WebRTC** voice chat (PeerJS)<br>• Google STUN servers for NAT traversal<br>• Mute / speaking indicators |
| **Authentication** | • Simple password demo (`test`)<br>• Per-session username for personalization |
| **UX Delight** | • Count-up animations for projections<br>• Dark theme, glass-morphism, responsive |
| **Security** | • HTTPS / WSS everywhere – Let’s Encrypt via Certbot |

---

## 🏗️ Architecture at a Glance

```

```
┌─────────────┐    WSS    ┌─────────────┐


Browser ─►│ Socket.IO   │◄──────────│  WS Server  │
│  Client     │           │  (Node)     │
└─────────────┘           └─────────────┘
▲                          ▲
│ WebRTC / PeerJS          │ REST (peers list)
│                          │
┌─────────────┐    HTTPS   ┌─────────────┐
│   PeerJS    │◄──────────►│ PeerServer  │
│   Client    │            │  (Node)     │
└─────────────┘            └─────────────┘

````

* **Frontend** – plain JS modules (`/js`)  
  * `socket.js` → WebSocket glue  
  * `peer.js`   → voice chat engine  
  * `charts.js` → Chart.js setup / live updates  
  * `dashboard.js`, `auth.js`, `app.js`  

* **Servers**  
  * **WebSocket**: `wss://ws.revenues.digital:2087`  
  * **PeerServer**: `https://webrtc.revenues.digital:2096`  

---

## 🚀 Getting Started

```bash
git clone https://github.com/your-org/revenues.digital.git
cd revenues.digital
# Static app – no build step required
````

1. **DNS / SSL**

   ```bash
   sudo certbot certonly --standalone -d revenues.digital \
                         -d ws.revenues.digital \
                         -d webrtc.revenues.digital
   ```

2. **Run servers (example)**

   ```bash
   # WebSocket
   PORT=2087 node server/ws-server.js

   # PeerServer
   peerjs --port 2096 --secure \
          --sslkey /etc/letsencrypt/live/revenues.digital/privkey.pem \
          --sslcert /etc/letsencrypt/live/revenues.digital/fullchain.pem
   ```

3. **Open** `https://revenues.digital`
   *Login with any username & password **test***.

---

## 🖼️ Screenshots

| Login Page                                    | Certbot Issuance                                | PeerServer CLI                                     |
| --------------------------------------------- | ----------------------------------------------- | -------------------------------------------------- |
| ![Login](https://i.ibb.co/CpSGGyt1/image.png) | ![Certbot](https://i.ibb.co/PZzDTCKc/image.png) | ![PeerServer](https://i.ibb.co/0pvpndvS/image.png) |

---

## ⚙️ Tech Stack

| Layer               | Tech                                                                                                                                                  |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Frontend            | Vanilla JS · [Chart.js](https://www.chartjs.org/) · [PeerJS](https://github.com/peers/peerjs) · [CountUp.js](https://github.com/inorganik/CountUp.js) |
| Real-Time Transport | **Socket.IO** (WebSocket-only)                                                                                                                        |
| Hosting             | **Amazon EC2** (*t2.micro*, Ubuntu 22.04 LTS)                                                                                                         |
| TLS                 | Let’s Encrypt / Certbot                                                                                                                               |

---

## 🆚 Why EC2 instead of Wix?

|          | **Amazon EC2**                                                                                                               | **Wix.com**                                                                         |
| :------- | :--------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| **Pros** | • Full customization & root access<br>• Real-time services, custom runtimes, any DB<br>• Separate frontend & backend freedom | • Rapid site building, zero server upkeep<br>• Built-in global CDN & 24 × 7 support |
| **Cons** | • Requires setup, monitoring, Linux chops<br>• Steeper learning curve                                                        | • No custom backend hosting<br>• Limited personalization beyond templates           |

> **Decision:** The dashboard demands bespoke WebSocket & WebRTC servers and fine-grained control over TLS, ports, and Linux packages—capabilities that EC2 offers while Wix does not.

---

## ✍️ Contributing

PRs are welcome! Please open an issue first to discuss major changes.

```bash
npm test        # run unit tests
npm run lint    # keep the style tidy
```

---

## 🛡️ License

[MIT](LICENSE)

---

## ☕ Acknowledgements

* [Socket.IO](https://socket.io/) – real-time engine
* [Chart.js](https://www.chartjs.org/) – charts
* [PeerJS](https://peerjs.com/) – WebRTC wrapper
* [CountUp.js](https://inorganik.github.io/countUp.js/) – animated counters
* Google STUN servers – ICE traversal
* Let’s Encrypt – free SSL

*Made with 💚 & **#14b8a6** in EC2’s free tier.*


