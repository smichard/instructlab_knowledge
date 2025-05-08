# Meshtastic

Meshtastic® is a project that enables you to use inexpensive LoRa radios as a long-range off-grid communication platform in areas without existing or reliable communications infrastructure. This project is 100% community driven and open source!

Meshtastic is a decentralized wireless off-grid mesh networking LoRa protocol. The main goal of the project is enabling low-power, long-range communication over unlicensed radio bands. It is designed around exchanging text messages and data in off-grid environments, with potential applications in IoT projects where a decentralized communication system is needed without existing infrastructure.
Meshtastic uses LoRa peer to peer (P2P), a long-range radio protocol, to form a mesh network by rebroadcasting messages to extend communication reach. Each device can connect with a single phone, enabling messaging in off-grid areas, making it useful for not only messages, but also data transmissions.

## Hardware

Meshtastic uses hardware development boards, like ESP32 and nRF52840, that support LoRa and BLE communication technologies, along with GNSS receivers. These devices enable seamless mobile app connectivity via Bluetooth or Wi-Fi, allowing long-range message retransmission across a mesh network using LoRa transceivers. This setup is ideal for developing communicators that don't rely on conventional infrastructure.
Commercial purpose built Meshtastic boards and kits are available.

## LoRa Topology

### Features

- Long range (331km record by MartinR7 & alleg)
- No phone required for mesh communication
- Decentralized communication — no dedicated router required
- Encrypted communication
- Excellent battery life
- Send and receive text messages between members of the mesh
- Optional GPS-based location features
- And more!

### How it Works

Meshtastic utilizes LoRa, a long-range radio protocol, widely accessible without needing additional licenses or certifications. Radios rebroadcast messages they receive, forming a mesh network where all members can receive messages—even over large distances. Each device supports a single phone connection.

## Contributors

Meshtastic is open source and volunteer-driven. You can contribute via GitHub, Discord, and Meshtastic Discussions.

## Getting Started

The project aims for a smooth onboarding process. If issues arise, please help improve the documentation or reach out through the community channels.

## Overview

Messages from the companion app are sent to the radio via Bluetooth, Wi-Fi/Ethernet, or serial. The radio broadcasts the message. If no acknowledgment is received, it retransmits up to three times.

Each radio caches about 30 packets. Old packets are replaced by new ones when full.

## What is a Mesh?

A Meshtastic mesh is a group of nodes that share the same LoRa parameters: spreading factor, frequency, and bandwidth. These are defined in radio presets.

### Channels

Channels sit on top of the radio mesh and are defined by a name and encryption key. The default channel (Channel 0) uses a blank name and the key `AQ==`. Nodes can join up to 8 channels and will receive all messages but may only read those for matching channels.

## Radio Settings

> Note: Meshtastic is **not** LoRaWAN, Helium, or TTN.

Meshtastic operates on the full LoRa spectrum available in each region. Legal limits vary by region and may change depending on licensing mode (e.g., `is_licensed = true`).

### Frequency Bands

#### Europe

- **433 MHz**: +10 dBm ERP, 433–434 MHz
- **868 MHz**: +27 dBm ERP, 869.40–869.65 MHz

#### North America

- **915 MHz ISM Band**: +30 dBm ERP, 902–928 MHz

### Frequency Slots

Each region supports a set number of slots based on preset configurations. For example, LongFast in North America supports 104 slots.

## Data Rates & Presets

| Preset                  | Data Rate  | SF  | Coding Rate | Bandwidth | Link Budget |
|-------------------------|------------|-----|--------------|-----------|-------------|
| Short Turbo             | 21.88 kbps | 7   | 4/5          | 500 kHz   | 140 dB      |
| Short Fast              | 10.94 kbps | 7   | 4/5          | 250 kHz   | 143 dB      |
| Short Slow              | 6.25 kbps  | 8   | 4/5          | 250 kHz   | 145.5 dB    |
| Medium Fast             | 3.52 kbps  | 9   | 4/5          | 250 kHz   | 148 dB      |
| Medium Slow             | 1.95 kbps  | 10  | 4/5          | 250 kHz   | 150.5 dB    |
| Long Fast               | 1.07 kbps  | 11  | 4/5          | 250 kHz   | 153 dB      |
| Long Moderate           | 0.34 kbps  | 11  | 4/8          | 125 kHz   | 156 dB      |
| Long Slow               | 0.18 kbps  | 12  | 4/8          | 125 kHz   | 158.5 dB    |

> Note: Actual performance may vary due to packet headers, retransmissions, and environmental factors.

## Custom Settings

You can apply custom LoRa settings. After changes, restart the device. A new channel key is generated and must be shared via QR or URL.

## Cryptography

Meshtastic supports:

- AES128 or AES256 (channel-level)
- Optional encryption disable (Ham mode)
- Public Key Cryptography (DMs only)
- QR code sharing of LoRa + channel settings

### Encryption Notes

- Default channel is weak (`AQ==`)
- Headers are unencrypted (for routing)
- Direct messages use PKC with digital signatures
- Admin messages are encrypted with session IDs (since v2.5.0)

## Broadcast Algorithm

### Layer 0: LoRa Radio

Handles transmission using a 16-symbol preamble and a physical header (`0x2B` sync word).

### Layer 1: Unreliable Messaging

Each packet includes:
- Destination NodeID
- Sender NodeID
- Unique Packet ID
- Flags
- Channel hash
- Next-hop + relay info
- Payload (protobuf encrypted)

### Layer 2: Reliable Messaging

Adds ACK/NAK and retry logic for neighbor messages. Broadcasts get implicit ACKs via observed rebroadcasts.

### Layer 3: Multi-Hop Messaging

Managed flooding: nodes with lower SNR rebroadcast first. Roles (e.g., router) affect behavior. Hop limit prevents infinite rebroadcasting.

### Next-Hop Routing

Introduced in v2.6 for direct messages. After successful delivery, routing is optimized to reduce redundancy.

## Broadcast Intervals

- **Telemetry**: every 30 min (default)
- **Position**: every 15 min (smart broadcast)
- **Node Info**: every 3 hours

> These intervals scale with mesh size to reduce congestion.

## Encryption & Security

Meshtastic is **not** as secure as TLS 1.3, WPA3, or Signal. It lacks:

- Perfect Forward Secrecy
- Channel message integrity verification
- Strong authentication (based on MAC address)

### Security Recommendations

- Avoid putting private channels on unattended nodes
- Change channel keys regularly
- Use latest firmware for best security
- Don't trust sender ID unless using DM with PKC

## Direct Messages

Now secured with:

- **Recipient’s Public Key** for encryption
- **Sender’s Private Key** for signing

Prevents eavesdropping and tampering.

## Admin Messages

Secured since v2.5.0 with:

- Stronger encryption
- Unique session IDs
- Replay protection

## Learn More

- 📘 [Channel Config](https://meshtastic.org/)
- 📘 [LoRa Config](https://meshtastic.org/)
- 💬 [Meshtastic GitHub](https://github.com/meshtastic/Meshtastic)


