# Git World Sync Mod

## Overview
This Minecraft Forge mod synchronizes the Minecraft world folder with a Git repository. It enables a turn-based hosting system, allowing multiple players to host the same world by pushing and pulling world state to/from a remote Git repository. This eliminates the manual transfer of world files between players who want to take turns hosting. All functionality is integrated directly within Minecraft's interface, requiring no external launcher.

## Planned Features -1
1. **Git Operations**:
   - Pull the latest world from the Git repository when the server starts.
   - Push the updated world to the Git repository when the server stops.
   - Branch management for different world versions or backups.

2. **Thread-Safe Locking**:
   - Ensures only one Git operation (push or pull) happens at a time using the `LockManager` class.
   - Prevents world corruption during Git operations.

3. **Integration with Minecraft Events**:
   - Automatically pulls the world from Git during the `ServerStartingEvent`.
   - Automatically pushes the world to Git during the `ServerStoppingEvent`.
   - Handles world saving before Git operations to ensure data consistency.

4. **Configuration System**:
   - Customizable settings for Git repository URL, credentials, branch name, etc.
   - In-game configuration UI for easy setup and management.
   - Support for configuration files for server-side settings.

## Planned Features -2
1. **Automated Backup System**:
   - Regular automatic commits at configurable intervals.
   - Backup rotation and management to prevent repository bloat.
   - Emergency backup before potentially destructive operations.

2. **Command System**:
   - In-game commands for manual Git operations (`/gitsync pull`, `/gitsync push`, etc.).
   - Permission system to control who can execute Git commands.
   - Status command to check synchronization state.

3. **Player Notification System**:
   - Notify players about Git operations (start, completion, errors).
   - Optional integration with Discord webhook for remote notifications.
   - Visual indicators for ongoing Git operations.

4. **Conflict Resolution**:
   - Smart handling of merge conflicts.
   - Option to restore from last known good state.
   - Conflict resolution UI for admin players.

5. **World Diff Viewer**:
   - View changes between world versions.
   - Track player modifications to the world.
   - Revert specific changes without full rollback.

6. **Host Transition Protocol**:
   - Coordinated handover between hosts.
   - Waiting period for new hosts to ensure clean transition.
   - Host queue management system.

7. **Performance Optimizations**:
   - Selective synchronization of only changed chunks.
   - Compression of world data before pushing to Git.
   - Multi-threaded Git operations where safe to do so.
   
8. **Host Optimization Feature**:
   - Automatic host switching based on system specifications and performance metrics.
   - Collection and reporting of host system specifications.
   - Real-time server performance monitoring and lag detection.
   - Seamless migration between hosts to minimize downtime.
   - Automatic client reconnection to new host.

## Planned Features -3
1. **Coordination Server Fallbacks**:
   - Support for custom coordination server URLs.
   - Automatic failover to backup coordination servers.
   - Federation support for decentralized coordination.
   - Offline mode operation with limited functionality.
   - Status indicators for coordination server health.

2. **Permission-Based Access Control**:
   - Owner-only, invite-only, or public access modes.
   - GUI-based permission management system.
   - YAML configuration option for permissions.
   - Role-based permissions (play, fork, host).
   - Automatic permission sync with coordination server.

3. **Federated Server Discovery**:
   - Cross-server world discovery protocol.
   - Browse public worlds across coordination servers.
   - Server metadata exchange (world size, player count).
   - ActivityPub-inspired federation handshakes.
   - Favorite/bookmark system for worlds.

4. **Metrics Dashboard**:
   - Real-time performance overlay.
   - FPS, TPS, player count, sync status display.
   - Optional anonymous telemetry submission.
   - Automated crash reporting with opt-in consent.
   - Historical performance tracking.

5. **Version Compatibility System**:
   - Minecraft version validation.
   - Mod and modloader version checking.
   - Automatic compatibility warnings.
   - Optional compatible modpack download.
   - Required mod list synchronization.

6. **Friends & Groups System**:
   - In-game friend management.
   - Optional integration with Discord/GitHub.
   - Group-based world sharing.
   - Friend online status indicators.
   - Group permissions management.

7. **Cold-Standby Hosting**:
   - Background world sync for standby hosts.
   - Automatic failover on host disconnection.
   - Seamless player transition between hosts.
   - Prioritized standby host selection.
   - Status indicators for standby readiness.

## Integration with Coordination Server
- API client to communicate with the coordination server
- Status updates to sync host changes with server
- Queue management integration
- Heartbeat implementation for server status
- Performance metrics reporting for host optimization
- System specification collection and comparison

## Project Structure
```
git-world-sync-mod/
├── build.gradle          # Gradle build file with dependencies for Forge and JGit
├── settings.gradle       # Gradle settings file
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── example/
│       │           └── gitworldsync/
│       │               ├── GitHandler.java           # Handles Git operations (pull/push)
│       │               ├── GitWorldSyncMod.java      # Main mod class
│       │               ├── LockManager.java          # Manages locking for Git operations
│       │               ├── config/
│       │               │   ├── ConfigHandler.java    # Handles mod configuration
│       │               │   └── ConfigScreen.java     # In-game configuration UI
│       │               ├── commands/
│       │               │   ├── GitSyncCommands.java  # Command registration
│       │               │   └── CommandHandler.java   # Command execution logic
│       │               ├── events/
│       │               │   ├── ServerEventHandler.java  # Server event listeners
│       │               │   └── PlayerEventHandler.java  # Player event listeners
│       │               ├── api/
│       │               │   ├── CoordinationClient.java  # API client for coordination server
│       │               │   ├── HostStatus.java          # Host status data structures
│       │               │   ├── QueueManager.java        # Queue interaction logic
│       │               │   ├── PerformanceReporter.java # System specs and performance reporting
│       │               │   ├── WorldRegistry.java       # Fetches available worlds from server
│       │               │   ├── AuthenticationService.java # Verifies legitimate Minecraft accounts
│       │               │   ├── TunnelService.java       # Manages tunnel endpoints
│       │               │   ├── FederationClient.java    # Handles cross-server discovery
│       │               │   ├── ServerHealthMonitor.java # Tracks coordination server availability
│       │               │   ├── AccessControlClient.java # Handles access right verification
│       │               │   └── TokenManager.java        # Manages Git write tokens
│       │               ├── performance/
│       │               │   ├── SystemSpecCollector.java # Collects host system specifications
│       │               │   ├── PerformanceMonitor.java  # Monitors TPS, memory usage, etc.
│       │               │   ├── MigrationManager.java    # Handles host migration process
│       │               │   ├── ReconnectionHandler.java # Manages client reconnection
│       │               │   ├── MetricsOverlay.java      # Real-time performance dashboard
│       │               │   ├── TelemetryManager.java    # Anonymous performance data handling
│       │               │   └── StandbyHostManager.java  # Manages cold-standby host system
│       │               ├── gui/
│       │               │   ├── LoginScreen.java        # Authentication with coordination server
│       │               │   ├── WorldListScreen.java    # Available shared worlds UI
│       │               │   ├── WorldInfoScreen.java    # Details about a specific world
│       │               │   ├── HostQueueScreen.java    # Queue management UI
│       │               │   ├── HostControlScreen.java  # Controls for current host
│       │               │   ├── PerformanceOverlay.java # Shows current performance metrics
│       │               │   ├── PermissionScreen.java   # World access permissions management
│       │               │   ├── FriendsScreen.java      # Friend management interface
│       │               │   ├── GroupScreen.java        # Group management interface
│       │               │   ├── ServerStatusScreen.java # Coordination server status view
│       │               │   ├── VersionCheckScreen.java # Version compatibility interface
│       │               │   ├── components/             # Reusable UI components
│       │               │   │   ├── WorldListEntry.java # List entry for available worlds
│       │               │   │   ├── PlayerQueueEntry.java # List entry for queued players
│       │               │   │   ├── ConfirmationPopup.java # Reusable confirmation dialog
│       │               │   │   ├── ToastNotification.java # Non-intrusive notifications
│       │               │   │   ├── FriendListEntry.java # Friend list entry component
│       │               │   │   └── ServerStatusIndicator.java # Server health indicator
│       │               │   └── widgets/               # Custom widget implementations
│       │               │       ├── CircularProgressBar.java # For loading indicators
│       │               │       ├── PlayerAvatar.java  # Shows player head with status
│       │               │       ├── WorldPreview.java  # Thumbnail/preview of world
│       │               │       ├── MetricsGraph.java  # Performance visualization widget
│       │               │       └── StatusBadge.java   # Status indicator badge
│       │               ├── tunnel/
│       │               │   ├── TunnelManager.java     # Orchestrates tunnel creation
│       │               │   ├── NgrokTunnel.java       # ngrok implementation
│       │               │   ├── CloudflaredTunnel.java # cloudflared implementation
│       │               │   └── LocalPortManager.java  # Handles port allocation
│       │               ├── session/
│       │               │   ├── WorldSession.java      # Tracks current world session
│       │               │   ├── HostSession.java       # Manages hosting session
│       │               │   ├── ClientSession.java     # Manages client connection session
│       │               │   ├── StandbySession.java    # Manages standby host session
│       │               │   ├── FallbackMode.java      # Coordination server fallback handling
│       │               │   └── TokenHeartbeatService.java # Handles token renewal heartbeats
│       │               ├── social/
│       │               │   ├── FriendsManager.java    # Manages friend relationships
│       │               │   ├── GroupManager.java      # Handles group creation and management
│       │               │   ├── OAuthHandler.java      # Handles external auth integrations
│       │               │   └── PresenceTracker.java   # Tracks online status of friends
│       │               ├── permissions/
│       │               │   ├── WorldAccess.java       # Defines access control levels
│       │               │   ├── PermissionManager.java # Handles permission management
│       │               │   ├── RoleDefinition.java    # Defines permission roles
│       │               │   └── YamlPermissionLoader.java # Loads permissions from YAML
│       │               ├── version/
│       │               │   ├── VersionChecker.java    # Handles version compatibility checks
│       │               │   ├── ModpackDownloader.java # Downloads compatible modpacks
│       │               │   └── RequiredModList.java   # Manages required mod information
│       │               └── util/
│       │                   ├── WorldHelper.java      # World manipulation utilities
│       │                   ├── GitCredentialStore.java  # Secure credential management
│       │                   ├── NotificationManager.java  # Player notifications
│       │                   ├── AccountVerifier.java   # Minecraft account verification
│       │                   ├── BinaryDownloader.java  # Downloads tunnel binaries if needed
│       │                   ├── FederationProtocol.java  # Federation handshake implementation
│       │                   └── GitAccessToken.java    # Git access token data structure
│       └── resources/
│           ├── META-INF/
│           │   └── mods.toml        # Mod metadata
│           └── assets/
│               └── gitworldsync/
│                   ├── lang/        # Localization files
│                   ├── textures/    # UI textures
│                   │   ├── gui/     # GUI textures and icons
│                   │   ├── icons/   # Status and action icons
│                   │   └── logos/   # Mod and feature logos
│                   └── sounds/      # Notification sounds
└── README.md                        # Documentation
```

## Dependencies
- **Minecraft Forge**: For modding capabilities and event system.
- **JGit**: Java implementation of Git for repository operations.
- **Cloth Config API**: For enhanced configuration UI.
- **OkHttp**: For API communication with coordination server.
- **OSHI**: For collecting detailed system hardware information.
- **ngrok/cloudflared Java Wrappers**: For tunnel management (or embedded binaries).
- **Minecraft Authentication Libraries**: For account verification.
- **GraphQL Java Client**: For federation API communication.
- **OAuth2 Client**: For social integration authentication.
- **YAML Parser**: For configuration and permission systems.

## Implementation Details
1. **Git Repository Management**:
   - Support for both HTTPS and SSH authentication.
   - Credential caching with secure storage.
   - Custom .gitignore to exclude unnecessary files from synchronization.

2. **World Data Handling**:
   - Proper handling of NBT data and binary files.
   - Appropriate timing for world saving before Git operations.
   - Selective synchronization of player data based on configuration.

3. **Error Recovery**:
   - Automatic rollback to previous state if Git operations fail.
   - Comprehensive logging system for troubleshooting.
   - Retry mechanisms for temporary network issues.

4. **Multiplatform Considerations**:
   - Cross-platform Git binary paths and commands.
   - OS-specific file locking and permissions handling.
   - Environment detection for appropriate configuration defaults.

5. **Host Optimization Implementation**:
   - Collect system specifications on startup (CPU, RAM, network bandwidth, disk speed).
   - Monitor real-time server performance metrics (TPS, memory usage, chunk load times).
   - Report metrics to coordination server with each heartbeat.
   - Implement graceful host transition protocol for seamless migration.
   - Provide client-side automatic reconnection to new host server.

6. **Integrated GUI System**:
   - Main menu button to access the Git World Sync interface.
   - Login screen for authentication with the coordination server.
   - World list screen showing available shared worlds with search/filter.
   - Host queue interface showing current and upcoming hosts.
   - One-click hosting activation with automatic tunnel setup.
   - Status indicators for synchronization operations.
   - Performance monitoring dashboard for hosts.

7. **Embedded Tunneling**:
   - Automatic download and setup of tunneling binaries (ngrok, cloudflared).
   - Port management for hosting Minecraft server.
   - Tunnel URL retrieval and reporting to coordination server.
   - Fallback mechanisms if one tunneling provider fails.
   - Bandwidth and connectivity monitoring.

8. **Session Management**:
   - Seamless transition between hosts with minimal disconnection.
   - State preservation during host migration (inventory, position, etc.).
   - Automatic reconnection to new host with progress indicator.
   - Play session statistics tracking.
   - Queue position updates and notifications.

9. **Coordination Server Failover**:
   - Primary and secondary coordination server configuration.
   - Automatic failover to secondary server on connection loss.
   - Manual override to force primary or secondary server usage.
   - Status indicators for current coordination server.

10. **Permission-Based Access Control**:
    - World access settings (public, private, friends-only).
    - In-game GUI for managing access permissions.
    - Integration with coordination server for permission syncing.
    - Support for role-based access control.

11. **Federated Server Discovery**:
    - Discovery of worlds on other servers.
    - Follow and unfollow servers for world updates.
    - Integration with ActivityPub for decentralized social features.
    - Server and world favoriting system.

12. **Metrics Dashboard**:
    - In-game dashboard for real-time metrics.
    - Optional telemetry data sharing.
    - Performance trend graphs and statistics.
    - Server health and status monitoring.

13. **Version Compatibility Checks**:
    - Check Minecraft, mod, and modloader versions.
    - Warn about potential compatibility issues.
    - Suggest or download compatible modpacks.
    - Sync required mods list with coordination server.

14. **Friends & Groups System**:
    - Manage friends and groups in-game.
    - Share worlds and access with friends or groups.
    - View friends' online status and current world.
    - Group-based permissions and settings.

15. **Cold-Standby Hosting**:
    - Keep backup hosts synchronized in the background.
    - Automatic switchover to backup host if primary fails.
    - Notify players of host change.
    - Prioritize hosts based on performance and reliability.

16. **Git-Based Access Control**:
    - Player access rights stored in coordination server metadata.
    - Permission levels for world access (read/join, write/host).
    - Access verification before performing Git operations.
    - Support for individual and group-based permissions.
    - Integration with friend system for access controls.

17. **Token-Based Git Authentication**:
    - Temporary Git write tokens issued by coordination server.
    - Token renewal via periodic heartbeats during active hosting.
    - Automatic token expiration when hosting ends.
    - Secure token storage using system keystore.
    - Token invalidation on session termination.

## Installation and Usage

### Installation
1. Install Minecraft Forge for your Minecraft version.
2. Place the GitWorldSync mod JAR in your mods folder.
3. Start Minecraft and authenticate with the coordination server.
4. Configure Git credentials and repository preferences (first-time setup).

### Basic Usage
1. **Starting or Joining a Shared World**:
   - Open the mod's main screen from the Minecraft main menu.
   - Browse available worlds or create a new shared world.
   - Select a world to view details (active host, queue status, etc.).
   - Click "Join" to connect if a host is active, or "Host" to become the host if none exists.

2. **Hosting a World**:
   - The mod automatically pulls the latest world state from Git.
   - Sets up tunneling for other players to connect.
   - When you're done hosting, click "End Session" to allow the next player to host.
   - World is automatically saved, committed, and pushed to Git.

3. **Queuing for Host**:
   - If world is currently hosted, join the queue to host next.
   - Receive notification when your turn is approaching.
   - Automatic host transition when current host ends their session.

### Advanced Usage
1. **Manual Synchronization**:
   - Use `/gitsync pull` to manually pull the latest world state.
   - Use `/gitsync push` to manually push the current world state.
   - Use `/gitsync status` to check the current synchronization status.

2. **Branch Management**:
   - Use `/gitsync branch create <name>` to create a new branch (world version).
   - Use `/gitsync branch switch <name>` to switch to a different branch.
   - Use `/gitsync branch list` to list available branches.

3. **Performance and Host Optimization**:
   - Use `/gitsync specs` to view and update your system specifications.
   - Use `/gitsync performance` to view current server performance metrics.
   - Use `/gitsync migrate` to manually trigger host migration (admin only).
   - Use `/gitsync host-candidates` to view potential better host candidates.

4. **GUI Customization**:
   - Use `/gitsync ui theme <theme-name>` to change UI appearance.
   - Use `/gitsync ui compact` to enable compact mode for smaller screens.
   - Use `/gitsync ui notifications <on/off>` to toggle in-game notifications.

5. **Coordination Server Management**:
   - Use `/gitsync server primary <url>` to set primary coordination server.
   - Use `/gitsync server secondary <url>` to set secondary coordination server.
   - Use `/gitsync server status` to check current server status.
   - Use `/gitsync server force-primary` to force use of primary server.
   - Use `/gitsync server force-secondary` to force use of secondary server.

6. **Permission Management**:
   - Use `/gitsync permissions <world>` to view and manage world permissions.
   - Use `/gitsync invite <player>` to invite player to world (if private).
   - Use `/gitsync kick <player>` to remove player from world.
   - Use `/gitsync promote <player>` to promote player to co-host.

7. **Friend and Group Management**:
   - Use `/gitsync friends` to view and manage friends list.
   - Use `/gitsync addfriend <player>` to send friend request.
   - Use `/gitsync removefriend <player>` to unfriend player.
   - Use `/gitsync groups` to view and manage groups.
   - Use `/gitsync creategroup <name>` to create a new group.
   - Use `/gitsync joingroup <name>` to join a group.
   - Use `/gitsync leavegroup` to leave current group.

8. **Federated Discovery**:
   - Use `/gitsync discover` to discover worlds on other servers.
   - Use `/gitsync follow <server>` to follow a server for updates.
   - Use `/gitsync unfollow <server>` to unfollow a server.
   - Use `/gitsync favorites` to view and manage favorite worlds/servers.

9. **Metrics and Performance**:
   - Use `/gitsync metrics` to view performance metrics.
   - Use `/gitsync telemetry <on/off>` to toggle telemetry data sharing.
   - Use `/gitsync crashes` to view crash reports and analytics.

10. **Version and Compatibility**:
    - Use `/gitsync version` to check Minecraft and mod version compatibility.
    - Use `/gitsync modpack` to download and install compatible modpacks.
    - Use `/gitsync required-mods` to view required mods for the world.

11. **Access Control Management**:
    - Use `/gitsync access <world> list` to view current access permissions.
    - Use `/gitsync access <world> grant <player> <permission>` to grant permissions.
    - Use `/gitsync access <world> revoke <player> <permission>` to revoke permissions.
    - Use `/gitsync access <world> public <true/false>` to toggle public access.
    - Use `/gitsync tokens` to view active Git tokens and their status.

## Development Status
### Things Done

### Things To Do
- Created the basic mod structure.
- Implemented `GitHandler` for Git operations (pull and push).
- Implemented `LockManager` for thread-safe locking.
- Integrated `GitHandler` and `LockManager` into `GitWorldSyncMod`.
- Added logic to pull the world from Git during the `ServerStartingEvent`.
- Implemented logic to push the world to Git when the server stops.
- Added error handling and logging for Git operations.
- Securely manage Git credentials (e.g., use environment variables or a config file).
- Implemented the configuration system with both file-based and UI options.
- Developed the command system for manual control of Git operations.
- Created the notification system for operation status updates.
- Built the automatic backup system with configurable intervals.
- Implemented conflict resolution strategies and UI.
- Developed the world diff viewer for change tracking.
- Optimized Git operations for large world files.
- Added API client for coordination server integration.
- Tested the mod in various Minecraft environments (single-player, LAN, dedicated server).
- Implemented system specification collection and reporting.
- Created server performance monitoring systems.
- Developed host migration protocol with the coordination server.
- Implemented client auto-reconnection to new host.
- Created GUI screens for all mod functionalities.
- Implemented coordination server fallback mechanism.
- Created permission-based access control system.
- Developed federated server discovery protocol.
- Built comprehensive metrics dashboard.
- Implemented version compatibility checking system.
- Created friends and groups social system.
- Developed cold-standby hosting functionality.
- Implement embedded tunneling systems.
- Develop account verification with Minecraft authentication.
- Create world list fetching from coordination server.
- Build host queue management interface.
- Implement player transition handling for host migration.
- Develop automatic binary downloader for tunnel providers.
- Create performance monitoring overlay.
- Test host migration in various network conditions.
- Test Git access token implementation across different networks.
- Implement robust token renewal and fallback mechanisms.
- Create UI for managing world access permissions.
- Test access control with large player communities.
- Implement token revocation for emergency situations.

## Notes for Future Development
- Ensure that the `build.gradle` file is up-to-date with the correct Forge and JGit versions.
- Use the `LockManager` to prevent race conditions during Git operations.
- Consider security implications of storing Git credentials.
- Test thoroughly with different world sizes to understand performance implications.
- Research Git LFS (Large File Storage) for handling large world files more efficiently.
- Consider adding support for differential backups to minimize repository size.
- Consider implementing a rolling host system for communities with many suitable hosts.
- Develop algorithms to balance host suitability with continuity (avoiding too-frequent switches).
- Add player voting system for manual host migration requests.
- Design GUI to match Minecraft visual style for consistency.
- Consider implementing custom resource packs for different UI themes.
- Explore WebRTC-based peer connections for potential lower-latency handoffs.
- Investigate possibility of partial world state transfers (only changed regions).
- Consider direct integration with popular hosting providers for temporary server hosting.
- Note that the external LauncherCLI component has been removed in favor of fully integrated in-game GUI.
- Implement multi-factor authentication for sensitive operations.
- Consider implementing token revocation lists for handling lost/stolen tokens.
- Research options for offline authentication when coordination server is unavailable.
- Design a permission hierarchy system for more granular access control.
- Implement audit logging for access control changes and token usage.

## Potential Challenges
- Handling large world files efficiently with Git.
- Managing merge conflicts in binary NBT data.
- Ensuring data integrity during synchronization.
- Dealing with network issues during Git operations.
- Supporting different Minecraft versions and mod compatibility.
- Ensuring seamless host transitions with minimal player disruption.
- Accurately measuring relevant system performance metrics.
- Handling spotty connectivity during host migration.
- Finding the right balance between automatic optimization and stability.
- Managing player expectations during host transitions.
- Finding reliable tunneling solutions that work across different network configurations.
- Managing the embedded binary downloads securely.
- Ensuring the GUI works across different Minecraft versions and mod loaders.
- Handling Minecraft authentication without compromising account security.
- Creating a seamless user experience during host transitions.
- Managing token renewal during unstable network connections.
- Handling token synchronization across multiple coordination servers.
- Balancing security with ease of use in the permission system.
- Dealing with clock synchronization issues affecting token expiration.
- Managing token revocation in offline scenarios.
