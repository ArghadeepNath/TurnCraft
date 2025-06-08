# Coordination Server for Git-Backed Minecraft World Hosting

## Overview
The Coordination Server is a lightweight web service that manages host transitions and player queuing for the Git-Backed Turn-Based Minecraft World Hosting System. It provides a central point for tracking who is currently hosting, who is in line to host next, and how to connect to the current host.

## Features

### 1. Host Status Management
- Track the current active host
- Store connection details (tunnel URL, IP address, port)
- Monitor host activity via heartbeats
- Automatically release inactive hosts

### 2. Player Queue System
- First-in, first-out (FIFO) queue for players waiting to host
- Fair distribution of hosting opportunities
- Notifications when a player's turn is approaching
- Priority queue options for special cases

### 3. API Endpoints
- **GET /status** - Returns information about current host (username, tunnel URL, uptime)
- **POST /host** - Claim host role with tunnel URL and credentials
- **POST /host/clear** - Release host role (current host only)
- **GET /queue** - View current queue of waiting players
- **POST /queue** - Join wait queue to become host later
- **DELETE /queue/{userId}** - Leave the queue
- **GET /next-host** - See who's next in line to host
- **POST /heartbeat** - Periodic ping to confirm host is still active

### 4. Security Features
- Authentication for sensitive operations
- Rate limiting to prevent abuse
- Host verification to prevent hijacking
- Token-based security for API access

### 5. Monitoring Dashboard
- Web interface showing current host status
- Queue visualization
- Historical hosting statistics
- System health indicators

### 6. Federation & Fallback System
- **GET /health** - Server health status endpoint for fallback checking
- **GET /federation/discover** - Discover other coordination servers
- **POST /federation/register** - Register as a federated server
- **GET /federation/worlds** - List public worlds across federated servers
- Inter-server communication protocol for world and host discovery
- Metrics for federation reliability and performance

### 7. Permission Management
- World access control (owner-only, invite-only, public)
- Invitation system for private worlds
- Role-based permissions (play, host, admin)
- API for permission verification and updates

### 8. Version Compatibility
- Track required Minecraft and mod versions per world
- Provide compatibility information to clients
- Optional modpack repository integration

### 9. Friends & Groups
- Friend relationship management
- Group creation and management
- Shared access controls for groups
- Online status tracking and notification

### 10. Performance Metrics
- Collect anonymous performance data (opt-in)
- Track host quality metrics
- Generate performance reports
- Identify optimal hosts based on metrics

### 11. Cold-Standby System
- Track potential standby hosts
- Coordinate standby activation on primary host failure
- Manage seamless transition protocol
- Monitor standby sync status

## Technical Implementation

### Stack Options
- **Option 1**: Python with Flask/FastAPI
  - Lightweight and easy to deploy
  - Good for rapid development
  - Excellent library support
- **Option 2**: Node.js with Express
  - Event-driven for good performance with many connections
  - JavaScript ecosystem benefits
  - Easy deployment options

### Persistence Layers
- **Lightweight Option**: SQLite for simple deployments
- **Scalable Option**: PostgreSQL for larger communities
- **In-Memory Option**: Redis for high-performance needs

### Deployment Options
- Docker container for easy setup
- Cloud deployment (AWS, Azure, GCP)
- Self-hosted on low-cost VPS
- Federation-ready deployment for distributed operation

## Project Structure
```
coordination-server/
├── src/
│   ├── app.js                # Main application (Node.js) OR app.py (Python)
│   ├── controllers/
│   │   ├── hostController.js  # Host management logic
│   │   ├── queueController.js # Queue management logic
│   │   ├── statusController.js # Status reporting
│   │   ├── federationController.js # Federation and discovery
│   │   ├── permissionController.js # World access permissions
│   │   ├── friendsController.js # Friend relationship management
│   │   ├── groupsController.js # Group management
│   │   ├── versionController.js # Version compatibility
│   │   ├── metricsController.js # Performance metrics
│   │   └── standbyController.js # Cold-standby management
│   ├── models/
│   │   ├── Host.js           # Host data model
│   │   ├── Queue.js          # Queue data model
│   │   ├── User.js           # User data model
│   │   ├── World.js          # World information model
│   │   ├── Permission.js     # Permission settings model
│   │   ├── FriendRelation.js # Friend relationship model
│   │   ├── Group.js          # Group definition model
│   │   ├── VersionInfo.js    # Version compatibility model
│   │   ├── PerformanceMetric.js # Performance data model
│   │   └── FederatedServer.js # Federated server model
│   ├── middleware/
│   │   ├── auth.js           # Authentication middleware
│   │   ├── rateLimiter.js    # Rate limiting
│   │   ├── logging.js        # Request logging
│   │   └── federationAuth.js # Federation authentication
│   ├── services/
│   │   ├── hostService.js    # Host business logic
│   │   ├── queueService.js   # Queue business logic
│   │   ├── notificationService.js # Notification handling
│   │   ├── federationService.js # Federation protocol handling
│   │   ├── permissionService.js # Access control logic
│   │   ├── friendService.js  # Friend management logic
│   │   ├── groupService.js   # Group management logic
│   │   ├── versionService.js # Version compatibility logic
│   │   ├── metricsService.js # Performance analytics
│   │   └── standbyService.js # Cold-standby coordination
│   └── utils/
│       ├── logger.js         # Logging utility
│       ├── config.js         # Configuration management
│       ├── federationProtocol.js # Federation protocol implementation
│       └── healthCheck.js    # Server health monitoring
├── public/                   # Static assets for dashboard
│   ├── css/
│   ├── js/
│   └── index.html            # Dashboard entry point
├── tests/                    # Automated tests
├── config/                   # Configuration files
├── package.json              # Dependencies (Node.js)
├── requirements.txt          # Dependencies (Python)
└── README.md                 # Documentation
```

## API Interface

### GET /status
Returns the current hosting status.

**Response:**
```json
{
  "hasActiveHost": true,
  "host": {
    "username": "player1",
    "tunnelUrl": "https://xyz123.ngrok.io",
    "startTime": "2023-07-01T15:30:00Z",
    "uptime": "2h 15m",
    "playerCount": 3
  },
  "queueLength": 2,
  "standbyHost": {
    "username": "player5",
    "syncStatus": "ready",
    "syncCompletionPercent": 100
  }
}
```

### GET /health
Server health status for fallback coordination.

**Response:**
```json
{
  "status": "healthy",
  "uptime": "5d 12h 30m",
  "activeHosts": 5,
  "activeWorlds": 12,
  "federationStatus": "connected",
  "version": "1.2.0"
}
```

### GET /federation/discover
Discover other coordination servers and their public worlds.

**Response:**
```json
{
  "servers": [
    {
      "url": "https://coord-server-2.example.com",
      "name": "Community Server 2",
      "status": "online",
      "worldCount": 8,
      "playerCount": 42
    },
    {
      "url": "https://coord-server-3.example.com",
      "name": "Bob's Server",
      "status": "online",
      "worldCount": 3,
      "playerCount": 12
    }
  ]
}
```

### GET /world/{worldId}/permissions
Get permissions for a specific world.

**Response:**
```json
{
  "worldId": "world-123",
  "accessMode": "invite-only",
  "owner": "player1",
  "invitedPlayers": [
    {
      "username": "player2",
      "permissions": ["play", "host"]
    },
    {
      "username": "player3",
      "permissions": ["play"]
    }
  ],
  "groups": [
    {
      "groupId": "group-456",
      "name": "Trusted Friends",
      "permissions": ["play", "host", "invite"]
    }
  ]
}
```

### GET /friends
Get the user's friend list.

**Response:**
```json
{
  "friends": [
    {
      "username": "player2",
      "status": "online",
      "currentWorld": "survival-world",
      "lastSeen": "2023-07-01T15:30:00Z"
    },
    {
      "username": "player3",
      "status": "offline",
      "lastSeen": "2023-06-30T22:15:00Z"
    }
  ],
  "pendingRequests": [
    {
      "username": "player4",
      "requestedAt": "2023-07-01T10:45:00Z"
    }
  ]
}
```

### GET /world/{worldId}/versions
Get version compatibility information for a world.

**Response:**
```json
{
  "worldId": "world-123",
  "minecraftVersion": "1.19.2",
  "modLoader": "forge-43.2.0",
  "requiredMods": [
    {
      "modId": "jei",
      "version": "10.2.1"
    },
    {
      "modId": "gitworldsync",
      "version": "1.5.0"
    }
  ],
  "recommendedModpack": {
    "name": "GitWorldSync Pack",
    "version": "2.3.1",
    "downloadUrl": "https://example.com/modpacks/gws-pack.zip"
  }
}
```

## Integration with Other Components

### Git World Sync Mod Integration
- Mod queries status endpoint to check if hosting is available
- Reports host status when server starts/stops
- Sends heartbeats while running
- Manages queue interactions directly through the mod's in-game interface
- Handles tunneling setup and management
- Updates host status and performance metrics
- Integrates with cold-standby system
- Manages federation discovery and fallbacks
- Implements permission and version compatibility checks
- Provides friends and group management UI

## Deployment Guide

### Prerequisites
- Node.js 14+ or Python 3.8+
- Database (SQLite for small deployments, PostgreSQL for larger ones)
- Reverse proxy (Nginx/Apache) for production deployments

### Basic Setup
1. Clone the repository
2. Install dependencies: `npm install` or `pip install -r requirements.txt`
3. Configure environment variables
4. Initialize database: `npm run init-db` or `python manage.py init-db`
5. Start server: `npm start` or `python app.py`

### Federation Setup
1. Configure server name and public URL
2. Set up federation credentials
3. Configure discovery settings
4. Add trusted federation peers (optional)
5. Enable federation endpoints: `npm run enable-federation` or `python manage.py enable-federation`

### Docker Deployment
```
docker build -t mc-coordination-server .
docker run -p 3000:3000 -v config:/app/config mc-coordination-server
```

## Development Roadmap

### Phase 1: Core Functionality
- Basic host tracking
- Simple queue system
- Essential API endpoints

### Phase 2: Enhanced Features
- User authentication
- Persistent queue storage
- Host statistics

### Phase 3: Advanced Features
- Web dashboard
- Notifications system
- Advanced queue management

### Phase 4: Federation & Resilience
- Server federation protocol
- Fallback mechanisms
- Health monitoring system

### Phase 5: Social & Permission Systems
- Friend and group management
- Advanced permission controls
- Social integration features

### Phase 6: Performance Optimization
- Metrics collection and analysis
- Standby host system
- Automated performance optimization

## Potential Challenges
- Handling network failures gracefully
- Preventing host abandonment
- Securing the API against abuse
- Scaling for larger communities
- Ensuring compatibility with mod's integrated functionality
- Maintaining federation consistency
- Handling permission conflicts across federated servers
- Optimizing cold-standby performance impact
- Balancing privacy with social features
