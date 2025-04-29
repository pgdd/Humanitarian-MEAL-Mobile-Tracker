# 📡 Humanitarian MEAL Mobile Tracker

> A mobile-first, encrypted humanitarian MEAL platform designed for conflict zones and unstable environments, built on decentralized infrastructure for maximum resilience and security.


## 🌍 Project Overview

"Technology designed for humanitarian operations must start by assuming the worst, the best, and remain unopinionated. Like money, medicine, or weaponry, its impact depends entirely on the hand that wields it — and still, it must work." – Pierre Giddio

Field teams in conflict zones often face unstable, censored, or unavailable Internet access. Traditional USB key data transfers are risky for field agents. This project provides a safer alternative: secure mobile data collection, local encryption, and radio-based transmission to decentralized, field-ready servers.

This project was not born from technical ambition, but from critical thinking combined with current best practices to ensure a smooth eventual adoption in the field. It anticipates the real conditions humanitarian workers face and grounds every technical choice in operational reality.

## 🛠️ Core Features

- Mobile-first, offline-capable MEAL data collection (WASH, Health, Protection, etc.)
- Full end-to-end encryption (libsodium / OpenPGP)
- Long-range radio transmission (LoRa / SDR / UHF options)
- Automated data decryption and injection into a local database (PostgreSQL / MongoDB)
- Fully decentralized: no cloud, no WhatsApp, no centralized APIs
- Optional AI-based feedback triage (TensorFlow Lite)

## 📡 Secure Radio Transmission Mode

### 📶 Transmission Range & Performance

The system supports two main radio technologies for resilient field transmission:

| Technology | Urban Range | Rural Range | Max w/ Repeater | Notes |
|------------|-------------|-------------|------------------|-------|
| **LoRa (868/915 MHz)** | 1–3 km | 10–20 km | 30–50 km | Encrypted short bursts; excellent power efficiency. |
| **SDR (Custom Protocols)** | 3–10 km | 20–50+ km | 100+ km (amplified) | High control; supports complex payloads and modulation schemes. |

These ranges are subject to terrain, antenna gain, weather, and transmission power. LoRa is preferred for lightweight, low-power, and covert deployment. SDR is used when more control or range is needed — e.g., over mountainous terrain or across borders.

All payloads are end-to-end encrypted before transmission, ensuring full confidentiality even if intercepted.

**Optional Module** designed for conflict environments:

```bash
+--------------------------+
| Mobile Device (Hardened) |
| - Data Collection        |
| - Encryption             |
| - Radio Transmission     |
+--------------------------+
                 ↓
+--------------------------+
| Receiver (Raspberry Pi)  |
| - Signal Capture         |
| - Decryption             |
| - Database Injection     |
+--------------------------+
                 ↓
+--------------------------+
| Field Dashboard          |
| - Activity Monitoring    |
| - Feedback Analysis      |
| - AI Classification      |
+--------------------------+
```

## 💡 Design Philosophy

> In humanitarian action, technology must serve dignity, safety, and operational reality first.

This project was conceived by asking critical questions:

- What if Internet access disappears or is intentionally disrupted?
- What if metadata exposure endangers vulnerable communities?
- How can field agents transmit data securely without revealing their presence or compromising beneficiaries?
- How do we defend communication against any institutions or governments with undisclosed access capabilities?

This app is built to answer those questions with:

- Zero-trust architecture: all data is encrypted end-to-end, even against compromised infrastructures.
- Independence from all centralized corporate platforms.
- Fully open-source components for transparency.
- Local ownership of encryption keys and operational data.
- Offline-first functionality ensuring resilience without permanent connectivity.

## 🛡 Technical and Security Rationale

### Why Not Just Next.js?

Next.js includes an API layer and is often sufficient for serving both frontend and backend in one place. However, in this project:

- **Node.js + Express.js** is used for **standalone backend logic** on decentralized devices (like radio receiver Pis) where frontend rendering isn't required.
- Next.js is used for **offline-capable PWA frontend**, especially for mobile agents.

This separation gives operational flexibility — a Raspberry Pi node might run just the Express logic (to ingest radio transmissions), while the Next.js app lives entirely offline on field phones.

It's not microservices for scaling, but **micro-isolation for security and resilience**.

| Layer | Technologies Used | Security Rationale |
|:------|:------------------|:-------------------|
| Mobile App | Next.js PWA | No mandatory account login, minimal tracking, offline-capable, open-source stack. |
| Mobile Device | GrapheneOS or hardened de-Googled Android | Removes telemetry; full control over permissions and encryption. iPhones, while secure in many ways, still rely on opaque systems and have been compromised in past high-profile cases (e.g., Pegasus). Jailbreaking may increase flexibility but reduces security if not perfectly maintained. |
| Backend | Node.js, Express.js – deployed only where radio reception and hardware integration are needed. Avoided if API logic can be embedded directly into the Next.js layer. Use cases include LoRa receiver nodes and air-gapped ingestion servers. | Lightweight, open, easy to audit, resilient API framework with encrypted endpoints. |
| Database | PostgreSQL / MongoDB | Encrypted at rest, fields containing sensitive information can be selectively encrypted (field-level encryption). |
| Encryption Libraries | libsodium / OpenPGP | State-of-the-art cryptographic standards trusted globally for defense and privacy operations. |
| Radio Transmission | LoRaWAN / SDR | Low-bandwidth, long-range, resilient, decentralized communication with optional asymmetric encryption at the transmission layer. |
| Notifications | Matrix Protocol / In-App Secure Messaging | Federated, encrypted, and metadata-minimized communications. No WhatsApp or Signal due to potential backdoor risks. |
| Hosting | Self-hosted Raspberry Pi nodes / decentralized field clusters | Removes reliance on Vercel, AWS, or any centralized infrastructure. Encourages geographic distribution of data sync nodes. |

## 🗺 Modular Architecture Diagram

The system is not only the result of engineering logic, but the product of **strategic thinking applied to humanitarian constraints** — acknowledging surveillance risks, degraded infrastructure, and the operational asymmetry between field workers and geopolitical actors.

```bash
  +-------------------------+
  | Mobile Data Collector  |
  | (GrapheneOS / PWA App) |
  | - Offline Form Capture |
  | - Local Encryption     |
  +-----------+------------+
              |
              | Encrypted Radio Transmission (LoRa / SDR)
              v
  +--------------------------+
  | Field Gateway Node       |
  | (Raspberry Pi + Radio)   |
  | - Radio Receiver         |
  | - Decryption (libsodium) |
  | - DB Injection (SQLite)  |
  +-----------+--------------+
              |
              | LAN / Mesh / USB
              v
  +--------------------------+
  | Local Dashboard Viewer   |
  | - PWA / Next.js Offline  |
  | - Optional AI Inference  |
  +--------------------------+
```

Each module operates independently but can sync when contact is restored. The architecture resists surveillance, censorship, and collapse. This is not just engineering — it’s operational strategy by design.

## 🧠 On Decentralization

This project embraces **network redundancy and decentralization**:
- Multiple self-hosted servers (Raspberry Pi or equivalent) distributed across field locations.
- Local syncing via radio, LoRa, or mesh networking.
- No single point of failure — if one node is lost, data can still be received by others.
- Data integrity is maintained via cryptographic signatures and verifiable sync chains.

This creates a resilient, censorship-resistant infrastructure suitable for contested and surveillance-heavy environments.

## 🎯 Purpose and Broader Applications

While the initial use case is grounded in the MEAL framework (Monitoring, Evaluation, Accountability, and Learning) — particularly communication between field agents and stakeholders — the platform is designed to be modular.

Potential extensions include:
- Health response tracking (patient data, triage logs)
- Cash distribution monitoring
- Protection case management
- Logistics and supply chain tracking
- Beneficiary feedback and complaint systems

Ultimately, the purpose is to create a secure, human-centered technology backbone for humanitarian missions — a system that assumes degraded environments, contested information spaces, and the need for absolute protection of both field staff and communities served.

## 🧠 Architectural Commitment

This system is designed with conviction: it will work. We start with assumptions grounded in field realities — degraded infrastructure, high surveillance risk, and unreliable power and connectivity — and architect from there.

This is not a speculative experiment or a beta SaaS prototype. It is a mission-critical design whose elements are based on sound, testable engineering principles and proven hardware constraints. While individual configurations may be refined over time, the architecture is already structured to meet its goals. Changes will only be driven by field validation, not technical ideology.

## 📦 Tech Stack

This architecture is exploratory and anticipatory — built not for polished deployment at scale, but to explore how a decentralized humanitarian communication system can function under real-world stress. Unlike traditional SaaS products, which can iterate with ease, systems deployed in humanitarian crises must work under constraint, with minimal fallback. Choices are thus based on resilience, clarity, and minimum viable operability.

| Layer | Tools |
|:------|:------|
| Frontend | Next.js, TailwindCSS (PWA support) – chosen for its ability to pre-render static pages, work offline, and deliver consistent UI on low-spec mobile hardware. Ideal for constrained field environments. |
| Backend | Node.js, Express.js |
| DB | PostgreSQL or MongoDB with encrypted fields |
| Encryption | libsodium / OpenPGP |
| Radio | LoRa / SDR on Raspberry Pi |
| AI (optional) | TensorFlow Lite |

## 🚀 Project Roadmap

**Phase 1 – Core System Architecture (Foundation)**
- [x] Project initialization and README
- [ ] Backend API (CRUD endpoints, encryption libraries integration)
- [ ] Frontend PWA mobile interface (Next.js offline-first setup)
- [ ] Secure local-only POST test (simulated radio transmission via localhost)

**Phase 2 – Radio Communication Layer**
- [ ] LoRa radio module integration (Raspberry Pi receiver with secure ingestion endpoint)
- [ ] SDR experimental setup (evaluate high-range transmission options)
- [ ] Encrypted transmission protocol design (packet structure, retries, integrity validation)

**Phase 3 – Data Management and Dashboard**
- [ ] Local dashboard PWA (Next.js static deployment on Raspberry Pi)
- [ ] Secure database implementation (PostgreSQL field encryption tuning)
- [ ] Basic field activity visualization

**Phase 4 – Mesh Networking and Decentralization**
- [ ] LoRa Mesh proof-of-concept using Meshtastic or disaster.radio framework
- [ ] Field node sync protocol design (conflict resolution, timestamping, sync encryption)
- [ ] Deployment of at least three redundant Pi nodes for decentralized sync test

**Phase 5 – Intelligent Field Data Processing (Optional Advanced Layer)**
- [ ] TensorFlow Lite local inference setup (classification of urgency of field reports)
- [ ] AI-assisted field feedback prioritization engine (offline-first)

**Phase 6 – Field Simulation and Validation**
- [ ] Simulated multi-agent field operation (full offline flow from collection to dashboard)
- [ ] Field-driven refinement (optimizing packet size, transmission intervals, offline UX)

**Phase 7 – Final Hardening and Open Release**
- [ ] Open-source project launch (GitHub, documentation, technical whitepaper)
- [ ] Modular deployment guides (mobile agent setup, Pi node receiver setup, mesh relay setup)
- [ ] Field security checklist and operational guidance documentation

**Phase 8 – Future Expansion: Decentralized Appstore Ecosystem**
- [ ] Design modular app container system (Next.js micro-frontends or offline-capable extensions)
- [ ] Create secure plugin architecture to allow adding new mission-specific modules (e.g., Health Reporting, Logistics Tracking, Protection Alerts)
- [ ] Develop a minimal "App Store" repository hosted locally or over mesh to distribute certified mission modules securely
- [ ] Design safe update mechanisms without needing central control (offline updates, QR code imports, LoRa syncs)

This roadmap reflects a commitment to build a resilient, secure, operational humanitarian communication system grounded in real-world strategy, scalable by design to broader decentralized humanitarian operations.

## 📚 Training Alongside

This project is developed in parallel with the [MEAL Essentials course on Kaya Connect](https://kayaconnect.org/course/view.php?id=706) to ensure that technical implementation is grounded in field-validated humanitarian principles.

## 📜 License?

After careful strategic thinking, this project is not licensed under a fully permissive MIT license. Given its potential dual-use nature (communications, encryption, mesh networking), a stronger ethical safeguard is necessary.

Thus, the project is licensed under the **GNU Affero General Public License v3.0 (AGPLv3)**. This ensures that:
- Any derivative work or deployment must also remain open and auditable.
- Commercial or state actors cannot privatize or weaponize the system without revealing their modifications.
- The original humanitarian intent remains structurally protected.

## ⚖️ Ethical Use Statement

This project was created to serve and protect humanitarian workers and vulnerable populations.

By using or modifying this software, you agree to respect the principles of neutrality, impartiality, independence, and humanity. Any use for surveillance, violence, political oppression, or military action is a betrayal of the spirit of this work.

---

## 🤝 Commitment to Humanitarian Principles

This project is built in full respect of the core humanitarian principles: neutrality, impartiality, independence, and humanity. It addresses technical and operational realities without political bias, aiming solely to protect field workers and the communities they serve, regardless of context or affiliation.

---