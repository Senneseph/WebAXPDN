# WebAXPDN (WebAXP Distribution Network)

WebAXPDN is the distribution network for WebAXP applications, providing a specialized infrastructure for distributing and consuming WebAXP applications and their assets. It is built on the WebAXD distribution layer and focuses specifically on the needs of WebAXP applications.

## Overview

WebAXPDN creates a peer-to-peer network for distributing WebAXP applications, allowing applications to be shared directly between users without relying on centralized servers. This approach provides several benefits:

- **Resilience**: The network continues to function even if some nodes go offline
- **Scalability**: The network can scale to handle large numbers of applications and users
- **Performance**: Applications can be loaded from nearby peers, reducing latency
- **Offline Support**: Applications can be accessed even when offline
- **Reduced Infrastructure Costs**: Less reliance on centralized servers reduces hosting costs

## Key Features

- **WebRTC-Based Distribution**: Uses WebRTC for peer-to-peer communication
- **Distributed Hash System**: Content-addressable storage for application assets
- **Automatic Content Discovery**: Automatically finds and retrieves application assets
- **Version Management**: Manages different versions of applications
- **Offline Support**: Applications can be accessed offline
- **Security**: End-to-end encryption and content verification
- **Integration with /dist**: Seamlessly integrates with the WebAXP build output

## Architecture

WebAXPDN consists of several key components:

### 1. Distribution Network

The peer-to-peer network that connects WebAXP applications:

- **Peer Discovery**: Finding other peers in the network
- **Connection Management**: Establishing and maintaining connections
- **Content Routing**: Routing content requests to the appropriate peers

### 2. Content Distribution

The system for distributing application content:

- **Content Chunking**: Splitting content into manageable chunks
- **Content Addressing**: Identifying content by its cryptographic hash
- **Content Replication**: Replicating content across the network
- **Content Verification**: Verifying the integrity of content

### 3. Application Management

Tools for managing WebAXP applications:

- **Application Packaging**: Packaging applications for distribution
- **Application Versioning**: Managing different versions of applications
- **Application Discovery**: Finding and retrieving applications
- **Application Updates**: Updating applications to new versions

### 4. Integration with /dist

Integration with the WebAXP build output:

- **Automatic Distribution**: Automatically distributing the contents of the /dist folder
- **Build Integration**: Integrating with the WebAXP build process
- **Development Mode**: Special handling for development mode

## Integration with WebAX Ecosystem

WebAXPDN integrates with other WebAX components:

- **WebAXP**: WebAXP applications are distributed through WebAXPDN
- **WebAXD**: WebAXPDN is built on the WebAXD distribution layer
- **jcule**: jcule can use WebAXPDN for distributing and consuming business logic
- **jellicule**: jellicule components can be distributed through WebAXPDN

## Usage

### Distributing a WebAXP Application

WebAXPDN is integrated with the WebAXP build process, so distributing an application is as simple as building it:

```bash
# Build and distribute the application
webaxp build --distribute
```

This will:

1. Build the application to the /dist folder
2. Package the contents of the /dist folder for distribution
3. Distribute the package through the WebAXPDN network
4. Return a distribution ID that can be used to access the application

### Consuming a Distributed Application

WebAXPDN provides a simple API for consuming distributed applications:

```javascript
import { loadApp } from '@net.webax/webaxpdn';

// Load a distributed application
const app = await loadApp('webaxpdn://distribution-id');

// Mount the application
app.mount('#app-container');
```

### Offline Support

WebAXPDN includes built-in support for offline access:

```javascript
import { cacheApp } from '@net.webax/webaxpdn';

// Cache an application for offline use
await cacheApp('webaxpdn://distribution-id');

// The application can now be accessed offline
const app = await loadApp('webaxpdn://distribution-id');
```

## Configuration

WebAXPDN can be configured through the WebAXP configuration file:

```javascript
// webaxp.config.js
module.exports = {
  // WebAXPDN configuration
  webaxpdn: {
    // Enable WebAXPDN
    enabled: true,
    
    // Distribution options
    distribution: {
      // Automatically distribute on build
      autoDist: true,
      
      // Distribution model
      model: 'p2p', // 'p2p' or 'distributed'
    },
    
    // Network options
    network: {
      // Signaling server
      signaling: 'wss://signaling.webax.net',
      
      // ICE servers
      iceServers: [
        { urls: 'stun:stun.webax.net:3478' },
        {
          urls: 'turn:turn.webax.net:3478',
          username: 'webax',
          credential: 'webax',
        },
      ],
      
      // Maximum number of peers
      maxPeers: 10,
      
      // Bootstrap nodes
      bootstrapNodes: [
        'webaxpdn://bootstrap-node-1',
        'webaxpdn://bootstrap-node-2',
      ],
    },
    
    // Storage options
    storage: {
      // Maximum storage size
      maxSize: '1GB',
      
      // Content expiration
      expiration: '30d',
      
      // Replication factor
      replicationFactor: 3,
    },
    
    // Security options
    security: {
      // Enable encryption
      encrypt: true,
      
      // Access control
      access: 'public', // 'public', 'private', or 'restricted'
    },
  },
};
```

## CLI

WebAXPDN includes a command-line interface for managing distributed applications:

```bash
# Distribute an application
webaxpdn distribute ./dist

# Load a distributed application
webaxpdn load webaxpdn://distribution-id --output ./app

# List distributed applications
webaxpdn list

# Get information about a distributed application
webaxpdn info webaxpdn://distribution-id

# Cache an application for offline use
webaxpdn cache webaxpdn://distribution-id

# Update an application to a new version
webaxpdn update webaxpdn://distribution-id
```

## API Reference

See the [API Reference](./docs/api.md) for detailed documentation of the WebAXPDN API.

## Examples

See the [Examples](./examples) directory for example applications using WebAXPDN.

## Implementation Details

### WebRTC Distribution

WebAXPDN uses WebRTC for peer-to-peer communication:

```javascript
// Connect to a peer
const connection = await webaxpdn.connect(peerId);

// Send data
connection.send(data);

// Receive data
connection.on('data', (data) => {
  console.log('Received data:', data);
});
```

### Distributed Hash System

WebAXPDN uses a distributed hash system for content-addressable storage:

```javascript
// Store content
const contentId = await webaxpdn.store(content);

// Retrieve content
const content = await webaxpdn.retrieve(contentId);
```

### Integration with /dist

WebAXPDN integrates with the WebAXP build output:

```javascript
// Distribute the contents of the /dist folder
const distributionId = await webaxpdn.distributeDir('./dist');

// The application can now be accessed using the distribution ID
const appUrl = `webaxpdn://${distributionId}`;
```

## Security Considerations

WebAXPDN includes several security features:

- **Content Encryption**: Content can be encrypted for secure distribution
- **Content Verification**: Content integrity is verified using cryptographic hashes
- **Access Control**: Access to content can be restricted to specific users
- **Peer Verification**: Peers can be verified to prevent malicious peers

## Performance Considerations

WebAXPDN is designed for performance:

- **Content Chunking**: Content is split into chunks for efficient transfer
- **Parallel Downloads**: Content can be downloaded from multiple peers in parallel
- **Caching**: Content is cached locally for faster access
- **Prefetching**: Content can be prefetched for improved performance

## Conclusion

WebAXPDN provides a powerful distribution network for WebAXP applications, enabling peer-to-peer sharing of applications and their assets. By leveraging WebRTC and a distributed hash system, WebAXPDN creates a resilient, scalable, and performant network for distributing web applications.
