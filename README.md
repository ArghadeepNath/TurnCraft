# 📂 Git-Backed Turn-Based Minecraft World Hosting System

<div align="center">

![Minecraft Version](https://img.shields.io/badge/Minecraft-1.20.1-green)
![Forge](https://img.shields.io/badge/Forge-Compatible-orange)
![Status](https://img.shields.io/badge/Status-In%20Development-blue)

*A modern solution for shared Minecraft world hosting - play together without being online together*

</div>

## 🌟 Overview

The **Git-Backed Turn-Based Minecraft World Hosting System** allows players to take turns hosting a shared Minecraft world without manually transferring world files. Using Git for world state synchronization and a coordination server to manage hosting transitions, players can seamlessly switch hosting duties while maintaining a persistent world state.

<div align="center">

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Player 1 PC    │     │  Player 2 PC    │     │  Player 3 PC    │
│  ┌───────────┐  │     │  ┌───────────┐  │     │  ┌───────────┐  │
│  │ Launcher  │──┼─────┼─▶│ Launcher  │──┼─────┼─▶│ Launcher  │  │
│  └───────────┘  │     │  └───────────┘  │     │  └───────────┘  │
│  ┌───────────┐  │     │  ┌───────────┐  │     │  ┌───────────┐  │
│  │Git World  │  │     │  │Git World  │  │     │  │Git World  │  │
│  │Sync Mod   │  │     │  │Sync Mod   │  │     │  │Sync Mod   │  │
│  └───────────┘  │     │  └───────────┘  │     │  └───────────┘  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
         │                      │                       │
         ▼                      ▼                       ▼
┌────────────────────────────────────────────────────────────────┐
│                      Git Repository                            │
└────────────────────────────────────────────────────────────────┘
                              ▲
                              │
                              ▼
┌────────────────────────────────────────────────────────────────┐
│                    Coordination Server                         │
└────────────────────────────────────────────────────────────────┘
```

</div>

## 🧩 System Components

The system consists of three main components that work together:

### 1. 🔄 Git World Sync Mod

A Minecraft Forge mod that handles Git synchronization of world data:

- **Automatic World Sync**: Pulls world data on server start, pushes on server stop
- **Thread-Safe Operations**: Ensures data integrity during Git operations
- **In-Game UI**: Complete GUI for managing shared worlds and hosting
- **Tunneling**: Embedded solution to make your server accessible to others
- **Performance Monitoring**: Tracks system metrics to optimize host selection
- **Host Migration**: Seamless transition between hosts with minimal disruption

### 2. 🌐 Coordination Server

A lightweight web service that manages host transitions and player queueing:

- **Host Status Management**: Tracks active hosts and connection details
- **Player Queue System**: Fair, FIFO-based queue for hosting turns
- **RESTful API**: Comprehensive endpoints for status checking and host management
- **Security Features**: Authentication, rate limiting, and token-based access
- **Monitoring Dashboard**: Web interface for system status visualization

### 3. 💻 In-Game Interface

Fully integrated UI within Minecraft for:

- Browsing available shared worlds
- Managing hosting sessions
- Viewing and joining the host queue
- Monitoring performance metrics
- One-click hosting activation/deactivation

## ✨ Key Features

- **No Manual File Transfers**: World state automatically synchronized via Git
- **Fair Turn-Based Hosting**: Queue system for organized hosting transitions
- **Smart Host Selection**: Optimizes for the best player host based on system specs
- **Seamless Transitions**: Minimal downtime during host switching
- **Built-in Tunneling**: No need for port forwarding or VPN solutions
- **Version Control**: Full history of world changes with branching support
- **In-Game Management**: No external tools required, everything accessible from Minecraft

## 🚀 Getting Started

### Prerequisites

- Minecraft (1.20.1 or compatible version)
- Minecraft Forge installed
- Git installed on your system
- Java 17 or newer


### Hosting a World (how it should work)

1. Open Minecraft and access the Git World Sync menu
2. Select "Host a World" option
3. Choose an existing world or create a new one
4. Click "Start Hosting"
5. The mod will:
   - Pull the latest world state (if joining an existing shared world)
   - Configure and launch the Minecraft server
   - Set up tunneling for external access
   - Register as the active host with the coordination server

### Joining a Hosted World

1. Open Minecraft and access the Git World Sync menu
2. View available shared worlds 
3. Select a world to see its status and current host
4. Click "Join World" to connect
5. Optionally, join the host queue to take a turn hosting later

**Key Dependencies**:
- JGit: Java implementation of Git
- OkHttp: API communication
- OSHI: System hardware information collection
- Tunneling solutions: ngrok/cloudflared

## 🛣️ Development Roadmap

### Phase 1: Core Functionality ✅
- Basic Git operations in mod
- Essential coordination server endpoints
- Simple host transitions

### Phase 2: Enhanced Features 🔄
- User authentication
- Improved queue management
- Web dashboard for monitoring
- System specification collection

### Phase 3: Advanced Features ⏳
- Smart host optimization
- Seamless player transitions
- Advanced conflict resolution
- Differential backups

## 🤝 Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) (link placeholder) for details on how to get started with development.

## 📊 System Requirements

### Minimum Requirements
- **Java**: JRE 17+
- **OS**: Windows 10+, macOS 10.15+, or Linux with kernel 4.19+
- **CPU**: Dual-core 2.0 GHz
- **RAM**: 4GB (8GB recommended)
- **Storage**: 1GB + world size
- **Network**: Stable broadband connection

### Recommended for Hosting
- **RAM**: 8GB+ 
- **CPU**: Quad-core 3.0+ GHz
- **Network**: 10+ Mbps upload speed
- **Storage**: SSD with 5GB+ free space

## ❓ Frequently Asked Questions

<details>
<summary><b>How large can the world be?</b></summary>
While there's no fixed limit, Git performance may degrade with extremely large worlds (10GB+). The system uses optimizations like selective synchronization to mitigate this issue.
</details>

<details>
<summary><b>What happens if a host disconnects unexpectedly?</b></summary>
The coordination server monitors host activity through heartbeats. If a host fails to send heartbeats, the server will mark the host as inactive after a timeout period and allow the next player in queue to take over.
</details>

<details>
<summary><b>Can I use mods with this system?</b></summary>
Yes! All players should have the same mod configuration. The system can synchronize modded world data as long as the mods are compatible with the server environment.
</details>

<details>
<summary><b>Does this work with modpacks?</b></summary>
Yes, the system is compatible with modpacks. For best results, all players should use the same exact modpack version.
</details>

<details>
<summary><b>What if there are Git conflicts?</b></summary>
The system includes conflict resolution strategies. For most conflicts, it will choose the most recent version or create a backup branch if needed.
</details>

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">
<i>Happy collaborative building! 🏗️ Never lose a world again.</i>
</div>
