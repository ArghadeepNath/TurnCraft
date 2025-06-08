# Project: Git-Backed Turn-Based Minecraft World Hosting System

## System Overview
This project enables players to take turns hosting a Minecraft world through a combination of:
1. A Minecraft Forge mod (Git World Sync Mod)
2. A coordination server for host management

Players can seamlessly transfer hosting duties without manually transferring world files. The system uses Git repositories to manage world state transfers and a coordination server to track who's currently hosting.

The system includes advanced features like server fallbacks, permission management, federation, metrics collection, version checks, social features, and cold-standby hosting to ensure optimal performance and user experience. Access control is managed through player-specific metadata stored on a Git-hased Player Access Metadata,hosting is done with temporary time-limited tokens providing secure Git access during hosting sessions.

## System Components

### 1. Git World Sync Mod (Minecraft Forge Mod)
- Handles synchronization between the local Minecraft world and a Git repository
- Automatically pulls/pushes world data during server start/stop
- Provides integrated in-game UI for world management and hosting controls
- Includes coordination server fallback mechanisms for resilience
- Features permission-based access control for worlds
- Implements metrics dashboard for performance monitoring
- Supports version compatibility checking for players
- Includes friends and group system for social interaction
- Provides cold-standby hosting for quick failover
- Uses token-based Git authentication for secure, time-limited repository access
- Implements heartbeat-based token renewal during active hosting
- See `GitWorldSyncMod.txt` for detailed implementation

### 2. Coordination Server
- Lightweight web server tracking host status and player queue
- Provides API endpoints for host management and status checking
- Supports federation protocol for decentralized operation
- Manages permission systems and access control
- Coordinates version compatibility checking
- Facilitates social features (friends, groups)
- Orchestrates cold-standby hosting system
- Issues and manages temporary Git access tokens for secure repository operations
- Stores world access metadata defining player permissions
- Processes token heartbeats to extend token validity during active hosting
- See `CoordinationServer.txt` for detailed implementation

## System Architecture
```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Player 1 PC    │     │  Player 2 PC    │     │  Player 3 PC    │
│  ┌───────────┐  │     │  ┌───────────┐  │     │  ┌───────────┐  │
│  │Git World  │──┼─────┼─▶│Git World  │──┼─────┼─▶│Git World  │  │
│  │Sync Mod   │  │     │  │Sync Mod   │  │     │  │Sync Mod   │  │
│  └───────────┘  │     │  └───────────┘  │     │  └───────────┘  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
         │                      │                       │
         ▼                      ▼                       ▼
┌────────────────────────────────────────────────────────────────┐
│                      Git Repository                            │◀─┐
└────────────────────────────────────────────────────────────────┘  │
                              ▲                                     │
                              │                                     │
                              ▼                                     │
┌────────────────────────────────────────────────────────────────┐  │
│                    Primary Coordination Server                 │  │
│                                                                │  │
│     ┌──────────────┐         ┌───────────────────────┐        │  │
│     │ Access       │◀───────▶│ Token Management      │────────┘  │
│     │ Metadata     │         │ (Issue, Renew, Revoke)│           │
│     └──────────────┘         └───────────────────────┘           │
│                                       ▲                          │
└───────────────────────────────────────┼──────────────────────────┘
                                        │
                                        │ Heartbeats
                                        │
                             ┌──────────┴───────────┐
                             │  Active Host Player  │
                             │  (Token Holder)      │
                             └─────────┬────────────┘
                                       │
                                       │ Token Expiration
                                       │ When Hosting Ends
                                       ▼
                             ┌─────────────────────────┐
                             │ Next Host in Queue      │
                             │ (Gets New Token)        │
                             └─────────────────────────┘
```

## Advanced Features

### Coordination Server Fallbacks
- Redundant coordination servers prevent single points of failure
- Automatic failover between coordination servers
- Federation protocol enables independently operated servers
- Health monitoring system validates server availability
- User notification of degraded or fallback operation modes

### Permission-Based Access
- Owner-only, invite-only, or public world access modes
- Player-specific permissions (play, host, admin)
- Group-based permissions management
- GUI and YAML configuration options
- Synchronization of permissions across coordination servers

### Federated Server Discovery
- Cross-server world and player discovery
- ActivityPub-inspired federation protocol
- Public world listing and browsing
- Metadata exchange between servers (world info, player counts)
- Federation health monitoring

### Metrics Dashboard
- Real-time performance display (FPS, TPS, memory, network)
- Player count and world statistics
- Optional anonymous telemetry reporting
- Performance analytics and optimization suggestions
- Sync status monitoring

### Version Compatibility
- Minecraft version validation
- Mod and modloader version checking
- Compatible modpack suggestions
- Automated compatibility warnings
- Required mod list management

### Friends & Group System
- Friend relationship management
- Online status tracking
- Group creation and management
- Shared world access for groups
- Optional authentication integration (Discord, GitHub)

### Cold-Standby Hosting
- Background world sync for potential standby hosts
- Automatic failover on host disconnection
- Seamless player transition between hosts
- Prioritized standby selection based on performance
- Status indicators for standby readiness

### Git-Based Access Control
- Player permissions stored as metadata on coordination server
- Read/join and write/host permission levels
- Permission checks before performing Git operations
- Integration with friends and groups system
- Support for public, private, and invite-only worlds

### Token-Based Authentication
- Temporary Git access tokens with limited time-to-live (TTL)
- Heartbeat-based token renewal for active hosting sessions
- Automatic token expiration when hosting ends
- Secure token storage and management
- Token revocation for emergency situations

## Workflow
1. **Initial Setup**:
   - Create a Git repository for the Minecraft world
   - All players install the mod, configure Git credentials
   - Deploy the coordination server
   - Configure initial world access permissions

2. **Hosting Process**:
   - Player checks coordination server for current host status via the mod's in-game UI
   - Player's access permissions are verified against server metadata
   - If no active host another player can claim hosting role
   - Coordination server issues a temporary Git write token to the player
   - Mod pulls latest world from Git using the token
   - Starts Minecraft server with integrated reverse tunnel
   - Registers as host with coordination server
   - Begins sending periodic heartbeats to extend token validity
   - Other players with access can connect via the tunnel URL

3. **Host Transition**:
   - Current host stops their server
   - Heartbeats cease and token is no longer renewed
   - Git World Sync Mod pushes world to Git with final token usage
   - Mod notifies coordination server that host role is released
   - Token is explicitly invalidated or expires shortly after
   - Next player in queue can become host and receive a new token
   - Cold-standby host may activate immediately for seamless transition

4. **Fallback Handling**:
   - Mod detects coordination server unavailability
   - Attempts connection to fallback servers in configured list
   - Falls back to read-only mode if all servers unreachable
   - Periodically checks for primary server restoration
   - Notifies user of fallback status and limitations

5. **Permission Management**:
   - World owner configures access permissions via GUI or YAML
   - Permissions are uploaded to coordination server
   - Players attempt to join are validated against permission rules
   - Friends and group members receive appropriate access levels
   - Permission changes are synchronized across federation

## Project Directory Structure
```
git-world-hosting-system/
├── git-world-sync-mod/         # Minecraft Forge mod
│   └── [See GitWorldSyncMod.txt for details]
├── coordination-server/        # Host management server
│   └── [See CoordinationServer.txt for details]
└── docs/                       # Documentation
    ├── Context.txt             # This file
    ├── GitWorldSyncMod.txt     # Mod documentation
    └── CoordinationServer.txt  # Server documentation
```

## Development Priorities
1. Build and test Git World Sync Mod
2. Implement basic coordination server
3. Integration testing of components
4. User documentation and tutorials
5. Implement coordination server fallbacks
6. Build permission-based access system
7. Develop federated server discovery
8. Create metrics dashboard
9. Implement version compatibility checks
10. Build friends and group system
11. Develop cold-standby hosting

## Contact
For questions or contributions, please contact the original developer or refer to the project repository.
