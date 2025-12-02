# EchoChat - Zero-Knowledge Messenger

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Android%20%7C%20iOS-blue?style=for-the-badge" alt="Platform">
  <img src="https://img.shields.io/badge/Encryption-X25519%20%2B%20AES--256--GCM-green?style=for-the-badge" alt="Encryption">
  <img src="https://img.shields.io/badge/Version-1.0.0-cyan?style=for-the-badge" alt="Version">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License">
</p>

<p align="center">
  <strong>🔒 True Privacy. Zero Data. No Compromise.</strong>
</p>

<p align="center">
  <a href="https://tta.lu/echochat.html">🌐 Website</a> •
  <a href="#download">📱 Download</a> •
  <a href="#how-it-works">🔐 How it Works</a> •
  <a href="#server-privacy--security">🛡️ Server Security</a> •
  <a href="#support-the-project">💜 Support</a>
</p>

---

## What is EchoChat?

EchoChat is a **zero-knowledge messenger** designed for those who take privacy seriously. Unlike other messaging apps that promise encryption but still collect metadata, EchoChat is architecturally designed so that **the server cannot read, store, or analyze anything**.

- ✅ **End-to-End Encryption** — Messages encrypted on your device
- ✅ **Zero Server Storage** — No database, no logs, RAM-only
- ✅ **No Metadata** — Server doesn't know who talks to whom
- ✅ **No Phone Number Required** — Connect via QR codes or Friend Codes
- ✅ **Ephemeral Sessions** — Auto-expire after 3 days
- ✅ **Open Architecture** — Transparent about how it works

---

## Download

### Android
📥 **[Download APK](https://github.com/omgnoe/echochat/releases/latest)** (Direct download, no Google Play required)

### iOS
🍎 **[Download on App Store](https://apps.apple.com/us/app/echochat-secure-messenger/id6755791156)**

---

## How it Works

### Architecture Overview

```
┌─────────────┐          ┌─────────────────┐          ┌─────────────┐
│  Client A   │◄────────►│   Relay Server  │◄────────►│  Client B   │
│             │          │                 │          │             │
│ • X25519    │   WSS    │ • RAM Only      │   WSS    │ • X25519    │
│ • AES-256   │ ══════►  │ • No Database   │ ◄══════  │ • AES-256   │
│ • Keychain  │          │ • No Logs       │          │ • Keychain  │
└─────────────┘          └─────────────────┘          └─────────────┘
     Keys stay                 Just relays              Keys stay
     on device              encrypted blobs             on device
```

### Cryptographic Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Key Exchange** | X25519 (ECDH) | Derives shared secret between peers |
| **Encryption** | AES-256-GCM | Symmetric encryption of messages |
| **Key Storage** | Platform Keychain | Secure storage (Android Keystore / iOS Keychain) |
| **Transport** | WSS (WebSocket Secure) | Encrypted communication channel |

### The Zero-Knowledge Promise

1. **Keys are generated locally** — Your private key never leaves your device
2. **ECDH Key Exchange** — Shared secret computed without server involvement
3. **Server sees only ciphertext** — Even if compromised, attacker gains nothing
4. **No user accounts** — Just random Friend Codes (e.g., `ECHO-A7B3C9D2`)
5. **Sessions auto-expire** — 3 days of inactivity = complete cleanup

---

## Technology Stack

### Mobile App (Flutter)

```
lib/
├── main.dart                 # App entry point
├── services/
│   ├── crypto_service.dart   # X25519 + AES-256-GCM encryption
│   ├── identity_service.dart # Key generation & secure storage
│   ├── ws_service.dart       # WebSocket client with reconnection
│   ├── session_service.dart  # Local session management
│   ├── friends_service.dart  # Friend list (encrypted storage)
│   └── notification_service.dart # Push notifications
├── screens/
│   ├── home_screen.dart      # Session list & creation
│   ├── chat_screen.dart      # E2E encrypted messaging
│   ├── friends_screen.dart   # QR scanning & friend management
│   └── account_setup_screen.dart # Initial identity creation
```

**Key Dependencies:**
- `cryptography` — X25519 & AES-GCM implementation
- `flutter_secure_storage` — Platform keychain access
- `web_socket_channel` — WebSocket communication
- `flutter_local_notifications` — Push notifications
- `mobile_scanner` — QR code scanning

### Server (Node.js / TypeScript)

```
server/
├── server.ts           # Main WebSocket server
├── session_manager.ts  # Session lifecycle (RAM-only)
└── group_manager.ts    # Group chat support (future)
```

**Server Characteristics:**
- **Zero persistence** — No database, no file storage
- **RAM-only sessions** — Everything lives in memory
- **Auto-cleanup** — Sessions expire after 3 days
- **Minimal logging** — Only errors, no access logs

---

## Security Features

### What the Server CANNOT Do
- ❌ Read message content
- ❌ Identify users by real identity
- ❌ Store conversation history
- ❌ Correlate who talks to whom over time
- ❌ Comply with data requests (no data exists)

### What the Server CAN Do
- ✅ Relay encrypted blobs between sessions
- ✅ Notify when someone joins/leaves a session
- ✅ Forward ping invitations (sender name only)
- ✅ Enforce session expiration

### Local Security
- 🔐 Private keys in platform Keychain/Keystore
- 🔐 Local data encrypted with AES-256
- 🔐 Friend list encrypted at rest
- 🔐 Session history encrypted locally

---

## Server Privacy & Security

We believe in full transparency about how we protect your privacy. Here's what our relay server does — and doesn't do.

### 🚫 What We DON'T Collect

| Data | Status |
|------|--------|
| IP Addresses | ❌ Not logged |
| Access Logs | ❌ Disabled |
| Message Content | ❌ Encrypted, unreadable |
| User Identities | ❌ No accounts, no tracking |
| Metadata | ❌ No timestamps per user |
| Device Fingerprints | ❌ Not collected |
| Chat Partners | ❌ No relationship mapping |

### 🛡️ Server Hardening

| Protection | Implementation |
|------------|----------------|
| **Encryption** | TLS 1.2/1.3 only — legacy protocols disabled |
| **DDoS Protection** | Cloudflare-proxied infrastructure |
| **Storage** | RAM-only — no database, no persistence |
| **Session Cleanup** | Automatic purge after 3 days inactivity |
| **IP Anonymization** | Backend never receives real client IPs |
| **Brute-Force Protection** | Automated blocking of malicious attempts |
| **Access Logging** | Completely disabled — nothing to subpoena |

### 🔐 Infrastructure Design

```
┌──────────┐      ┌─────────────────┐      ┌──────────────┐
│   User   │ ───► │   Cloudflare    │ ───► │ Relay Server │
└──────────┘      │                 │      │              │
                  │ • DDoS Shield   │      │ • No Logs    │
                  │ • IP Hidden     │      │ • RAM Only   │
                  │ • TLS 1.3       │      │ • Auto-Purge │
                  └─────────────────┘      └──────────────┘
```

### 📜 Our Privacy Promise

1. **We cannot read your messages** — End-to-end encrypted with keys only you control
2. **We cannot identify you** — No IPs logged, no accounts, no tracking
3. **We cannot comply with data requests** — No data exists to hand over
4. **We cannot sell your data** — There's nothing to sell
5. **We cannot be compromised meaningfully** — Even with server access, messages remain encrypted

> *"The best way to protect data is to never collect it."*

---

## Features

### Current (v1.0)
- [x] End-to-end encrypted 1:1 messaging
- [x] QR code friend exchange
- [x] Ephemeral sessions (3-day expiry)
- [x] Ping notifications ("X wants to chat")
- [x] Local message history (encrypted)
- [x] Cross-platform (Android + iOS)

### Planned
- [ ] Group chats (encrypted)
- [ ] File sharing (encrypted)
- [ ] Voice messages
- [ ] Disappearing messages (timer)
- [ ] Desktop app (Windows/macOS/Linux)
- [ ] Self-hosted server option

---

## How to Use

1. **Download & Install** — Get the APK or join TestFlight
2. **Create Identity** — Choose a nickname (no email/phone needed)
3. **Add Friends** — Scan QR codes or share your Friend Code
4. **Start a Session** — Create a chat and share the Session ID
5. **Chat Securely** — Messages are end-to-end encrypted

---

## Support the Project

EchoChat is free and always will be. There's no tracking, no ads, and no monetization of your data — because we don't have your data.

If you find EchoChat useful and want to support ongoing development and server costs, consider donating:

### Solana (SOL)
```
4twPuihvSABNLwDq3tvz3dFigx6X7EABBqrhfaSH4hmq
```

---

## Contributing

This is version 1.0 — the first public release! 

**We'd love your feedback:**
- 🐛 Found a bug? [Open an issue](https://github.com/omgnoe/echochat/issues)
- 💡 Have a feature idea? Let us know!
- 🔒 Security concern? Please report responsibly

---

## Links

- 🌐 **Website:** [https://tta.lu/echochat.html](https://tta.lu/echochat.html)
- 📱 **Android APK:** [Releases](https://github.com/omgnoe/echochat/releases)
- 🍎 **iOS App Store:** [Download](https://apps.apple.com/us/app/echochat-secure-messenger/id6755791156)
- 📊 **Server Status:** [Status Page](https://stats.uptimerobot.com/FXUJMYs59c)

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

<p align="center">
  <strong>Made with ❤️ by <a href="https://tta.lu">TTA</a></strong><br>
  <em>Privacy is a right, not a feature.</em>
</p>
